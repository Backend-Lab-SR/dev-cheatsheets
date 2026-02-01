# Java Cheatsheet

## Compilation and Execution

```bash
# Compile Java file
javac MyClass.java

# Run Java program
java MyClass

# Java 11+: compile & run in one step
java MyClass.java

# Compile with classpath
javac -cp ".:lib/*" MyClass.java

# Run with classpath
java -cp ".:lib/*" MyClass

# Compile multiple files
javac *.java

# Run with specific JVM options
java -Xmx512m -Xms256m MyClass
```

## JAR Operations

```bash
# Create JAR file
jar cvf myapp.jar *.class

# Create executable JAR with manifest
jar cvfm myapp.jar MANIFEST.MF *.class

# Extract JAR file
jar xvf myapp.jar

# List JAR contents
jar tf myapp.jar

# Run JAR file
java -jar myapp.jar
```

## Maven Integration

```bash
# Compile with Maven
mvn compile

# Clean + build
mvn clean package

# Run tests
mvn test

# Run with Maven
mvn exec:java -Dexec.mainClass="com.example.MyClass"

# Package and run
mvn package && java -jar target/myapp.jar
```

## Debugging

```bash

# Enable remote debugging (Java 9+)
java -agentlib:jdwp=transport=dt_socket,server=y,suspend=n,address=*:5005 MyClass

# Enable GC & heap logging (replaces deprecated PrintGC* flags)
java -Xlog:gc*,gc+heap=info MyClass

# Enable GC logging and write to file (recommended)
java -Xlog:gc*,gc+heap=info:file=gc.log:time,uptime,level MyClass

# Heap dump (requires full JDK)
jmap -dump:format=b,file=heap.bin <pid>

```

## Classpath Management

```bash

# Set classpath (Linux / macOS)
export CLASSPATH=/path/to/classes:/path/to/lib.jar

# Set classpath (Windows - Command Prompt)
set CLASSPATH=C:\path\to\classes;C:\path\to\lib.jar

# Check classpath (Linux / macOS)
echo $CLASSPATH

# Check classpath (Windows)
echo %CLASSPATH%

# Preferred: specify classpath at runtime (recommended over CLASSPATH env var)
java -cp ".:lib/*" MyClass        # Linux / macOS
java -cp ".;lib/*" MyClass        # Windows

# Find compiled class files
find . -name "MyClass.class"
```

## Process Management

```bash
# Find Java processes
jps -l

# Show Java process details
jps -v

# Interactive process viewer
jconsole

# Kill Java process
kill <pid>

# Force kill
kill -9 <pid>
```

## JVM Monitoring

```bash
# JVM info
# JVM version and runtime info
java -version

# Show JVM heap-related options (Java 9+)
java -XX:+PrintFlagsFinal -version | grep -Ei "heap|metaspace"

# Thread dump (requires full JDK)
jstack <pid>

# Garbage collection & memory statistics (every 1s)
jstat -gc <pid> 1000

# JVM command interface (recommended for modern diagnostics)
jcmd <pid> VM.flags
jcmd <pid> GC.heap_info
jcmd <pid> GC.run

```

## Common JVM Flags

```bash
# Heap size
-Xmx2g                  # Maximum heap size
-Xms1g                  # Initial heap size

# Garbage collection
-XX:+UseG1GC            # Use G1 garbage collector (default in modern Java)
-XX:+UseParallelGC      # Use Parallel GC

# Remote debugging (Java 9+)
-agentlib:jdwp=transport=dt_socket,server=y,suspend=n,address=*:5005

# GC & heap logging (Java 9+)
-Xlog:gc*,gc+heap=info:file=gc.log:time,uptime,level

# Example: Run an executable JAR with tuned heap settings, G1 GC,
# detailed GC & heap logging to a file, and remote debugging enabled
# (suitable for development or staging environments)
java \
  -Xms1g -Xmx2g \
  -XX:+UseG1GC \
  -Xlog:gc*,gc+heap=info:file=gc.log \
  -agentlib:jdwp=transport=dt_socket,server=y,suspend=n,address=*:5005 \
  -jar myapp.jar
```
