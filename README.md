# tinyFTP
Implementation of a tiny FTP client and server

## Basic command

ftp [hostname | ip-address]

- **GET**:     get [remote-file] [local-file]
- **MGET**:    mget [remote-files]
- **PUT**:     put [local-file] [remote-file]
- **MPUT**:    mput [local-files]
- **EXIT**:    ftp> bye(or quit)

##  FTP model

```
                                          -------------
                                          |/---------\|
                                          ||   User  ||    --------
                                          ||Interface|<--->| User |
                                          |\----^----/|    --------
                ----------                |     |     |
                |/------\|  FTP Commands  |/----V----\|
                ||Server|<---------------->|  Client ||
                ||  PI  ||  FTP Replies   ||    PI   ||
                |\--^---/|                |\----^----/|
                |   |    |                |     |     |
    --------    |/--V---\|      Data      |/----V----\|    --------
    | File |<--->|Server|<---------------->|  Client |<--->| File |
    |System|    || DTP  ||   Connection   ||   DTP   ||    |System|
    --------    |\------/|                |\---------/|    --------
                ----------                -------------

                Server-FTP                   Client-FTP

    NOTES: 1. The data connection may be used in either direction.
           2. The data connection need not exist all of the time.

                    Figure 1  Model for FTP Use

```

In the model described in Figure 1, the user-protocol interpreter
initiates the control connection.  The control connection follows
the Telnet protocol.  At the initiation of the user, standard FTP
commands are generated by the user-PI and transmitted to the
server process via the control connection.  (The user may
establish a direct control connection to the server-FTP, from a
TAC terminal for example, and generate standard FTP commands
independently, bypassing the user-FTP process.) Standard replies
are sent from the server-PI to the user-PI over the control
connection in response to the commands.

The FTP commands specify the parameters for the data connection
(data port, transfer mode, representation type, and structure) and
the nature of file system operation (store, retrieve, append,
delete, etc.).  The user-DTP or its designate should "listen" on
the specified data port, and the server initiate the data
connection and data transfer in accordance with the specified
parameters.