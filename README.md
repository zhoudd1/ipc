# IPC

IPC is a simple interprocess comunication library written in C. The IPC library wraps named pipes and works in posix compatible opperating systems (Support for windows is comming). The IPC library is based on sending messages through a connection and subscribing to message subjects on the other end of a connection.

Send message example:
```C
#include <string.h>
#include "ipc.h"

int main (int argc char* argv[]){
    Connection* conn = connectionConnect("ExampleConnectionName", CONN_TYPE_SUB);
    char* data = "Example message data";
    Message* msg = messageCreate(data, strlen(data) +1);
    messageSetSubject(msg, "Example Subject");
    connectionSend(conn, msg);
    messagedestroy(msg);
    connectionDestroy(conn); 
}
```

Recive message example:
```C
#include "ipc.h"

int recieved = 0;
void callback(Message* msg){
    printf(msg->data);
    recieved = 1;
}

int main (int argc char* argv[]){
    Connection* conn = connectionCreate("ExampleConnectionName", CONN_TYPE_SUB);
    connectionSubscribe(conn, "Example Subject");
    connectionSetCallback(conn, callback);
    connectionStartAutoDispatch(conn);
    
    while(!recieved){
        usleep(100);
    }
    
    connectionStopAutoDispatch(conn);
    connectionClose(conn);
    connectionDestroy(conn);
}
```

There is also a java binding for the IPC library. The Java API is similar to the C API.

# Building
#### Build All
run ```./build.sh``` in the root directory.
#### IPC Library
From the src directory run ```make```
#### JNI Library
Export your OS name to the JAVA_ARCH enviornment variable ex. linux32 linux64 darwin. There should be a folder with this name in $JAVA_HOME/include/$JAVA_ARCH

From the jni directory run ```make```
#### Linux Examples
From the examples/linux directory run ```make```
#### Java Examples
From the examples/java directory run ```make```
