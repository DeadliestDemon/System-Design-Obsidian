## Monitoring:

The modern economy depends on the continual operation of IT infrastructure. Such infrastructure contains hardware, distributed services, and network resources. These components are interlinked in such infrastructure, making it challenging to keep everything functioning smoothly and without application downtime.

Components can run into failures, response latency overshoot, overloaded or unreachable hardware, and containers running out of resources, among others. Multiple services are running in such an infrastructure, and anything can go awry.

Monitoring helps in analyzing such complex infrastructure where something is constantly failing. Monitoring distributed systems entails gathering, interpreting, and displaying data about the interactions between processes that are running at the same time. It assists in debugging, testing, performance evaluation, and having a bird’s-eye view over multiple services.

## How will we design a distributed monitoring system?

### 1. **[[Monitoring Server-side Errors]]**

1. **Designing a Monitoring System**: Define the requirements and high-level design of the monitoring system.

2. **A Detailed Design of the Monitoring System**: Go into the details of designing a monitoring system, and explore the components involved.

3. **Visualize Data in a Monitoring System**: Learn a unique way to visualize an enormous amount of monitoring data.

### 2. **[[Monitor Client-side Errors]]**

1. **Focus on Client-side Errors**: Get introduced to client-side errors and why it’s important to monitor them.

2. **Design a Client-side Monitoring System**: Learn to design a system that monitors the client-side errors.

### Server Side v/s Client Side Errors:

| Aspect                 | Server-Side Errors                                 | Client-Side Errors                                 |
|------------------------|----------------------------------------------------|----------------------------------------------------|
| Definition             | Errors that occur on the server during processing. | Errors that occur on the client-side (user's device).|
| Responsibility         | Usually related to server logic, databases, etc.   | Typically caused by browser, application code, etc.|
| Examples               | 500 Internal Server Error, Database Connection Failures, Server Overloads | 404 Not Found, Validation Errors, JavaScript Errors |
| Impact                 | Can affect multiple users simultaneously.          | Usually affect individual users.                  |
| Error Visibility      | May not be immediately visible to end-users.       | Immediately visible to the user.                  |
| Debugging Difficulty   | Can be challenging due to complex server systems.  | Easier to debug using browser dev tools.         |
| Network Dependency     | Can be influenced by network latency and stability.| May occur independently of network issues.        |
| Data Validation        | Typically involves validating data received from clients and responding accordingly. | Generally involves user input validation and handling erroneous input. |
| Data Transfer          | Involves transferring processed data to clients.   | Occurs while sending data from clients to servers. |
| Error Handling         | Server-side errors require proper error handling mechanisms to prevent crashes and data corruption. | Client-side errors need careful error handling to maintain a smooth user experience. |
| Mitigation Strategies  | Load balancing, server redundancy, error logging, disaster recovery planning. | Client-side validation, user-friendly error messages, responsive UI design. |
