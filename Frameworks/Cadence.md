# Cadence

### What is Cadence

- A task orchestrator
- A framework enables coordinating tasks
- Using Cadence can execute a logical flow of tasks asynchronously or synchronously.
- Cadence consists of a programming framework (or client library) and a managed service (or backend).

#### How can it be used for ISL and CWL

- Step functions
- Engines
- **TO-DO**

#### User cases
- Case1: different chains of workflows, docker images
- Case2: user decisions with branches

### How Cadence Works

- In Cadence, you can code the logical flow of events separately as a workflow and code business logic as activities. The workflow identifies the activities and sequences them, while an activity executes the logic.


### Get Started

#### Start the Cadence server locally

Install Cadence: followed the Cadence build instructions [Cadence build steps](https://github.com/uber/cadence/blob/master/CONTRIBUTING.md), but here are some notes for several steps that are not explicit in the original README file.

##### Steps:
- Development Environment 
- Checking out the code
- Dependency management
- Building
- Configure Mysql


1. Development Environment 

      - Go. Install on OS X with ```brew install go```

    - Setting go environment, setting bash_profile, GOPATH, GOROOT, PATH, etc
    ```
    vim ~/.bash_profile
    ```
    Edit the bash_profile
    ```
    export GOPATH=$HOME/go-directory # change to your own go path
    export GOROOT=/usr/local/opt/go/libexec
    export GOBIN=$GOPATH/bin
    export PATH=$PATH:$GOPATH/bin
    export PATH=$PATH:$GOROOT/bin
    ```



2. Checking out the code
      ```      
      cd $GOPATH
      git clone https://github.com/uber/cadence.git src/github.com/uber/cadence
      cd $GOPATH/src/github.com/uber/cadence
      ```
3. Dependency management

      run ```dep ensure```.

      If dep not installed, run 
      ```
      $ brew install dep
      $ brew upgrade dep
      ```
      

4. Building
      Compile the cadence service and helper tools without running test:
      ```
      make bins
      ```

    When doing ```make bins```, if it promotes: golint command not found. run this command and re-run the build:
    ```
    go get -u -v golang.org/x/lint/golint
    ```

5. Configure Mysql

      **Install cadence schema - mysql (use mysql 5.7)**
- Installation and start mysql
    ```
    brew install mysql@5.7
    brew tap homebrew/services
    brew link mysql@5.7 --force
    mysql -V
    brew services start mysql
    ```
    For more information, follow [brew install mysql 5.7](https://gist.github.com/operatino/392614486ce4421063b9dece4dfe6c21)

- Create user and password in Mysql and grant privileges
    ```
    mysql -u root
    CREATE USER 'uber'@'localhost' IDENTIFIED BY 'uber';
    GRANT ALL PRIVILEGES ON *.* TO 'uber'@'localhost' IDENTIFIED BY 'uber';
    FLUSH PRIVILEGES;
    ```

    **NOTES**: In the Makefile, we can not sucessfully compile the install-schema-mysql section. It is because the we should set a default username for database in ~/.my.cnf, or add ``` -u '$username' -pw '$password' ``` to the makefile. (For original Cadence project, they use uber/uber in build version, and root/root in docker version, within development_mysql.yaml file), so modify the Makefile as follows,
    ```
    install-schema-mysql: bins
	./cadence-sql-tool --ep 127.0.0.1 -u uber -pw uber create --db cadence
	./cadence-sql-tool --ep 127.0.0.1 -u uber -pw uber --db cadence setup-schema -v 0.0
	./cadence-sql-tool --ep 127.0.0.1 -u uber -pw uber --db cadence update-schema -d ./schema/mysql/v57/cadence/versioned
	./cadence-sql-tool --ep 127.0.0.1 -u uber -pw uber create --db cadence_visibility
	./cadence-sql-tool --ep 127.0.0.1 -u uber -pw uber  --db cadence_visibility setup-schema -v 0.0
	./cadence-sql-tool --ep 127.0.0.1 -u uber -pw uber --db cadence_visibility update-schema -d ./schema/mysql/v57/visibility/versioned
    ```
    
- Install cadence schema
    ```
    cd $GOPATH/github.com/uber/cadence
    make install-schema-mysql
    ```
- Start cadence server
    ```
    cd $GOPATH/github.com/uber/cadence
    cp config/development_mysql.yaml config/development.yaml
    ./cadence-server start --services=frontend,matching,history,worker
    ```
- Output 
    The output looks like the follows,
    ```
    2019/06/19 14:16:52 Loading config; env=development,zone=,configDir=./config
    2019/06/19 14:16:52 Loading configFiles=[./config/base.yaml ./config/development.yaml]
    2019/06/19 14:16:52 config=
    
    2019/06/19 14:16:52 error creating file based dynamic config client, use no-op config client instead. error: stat : no such file or directory
    {"level":"info","ts":"2019-06-19T14:16:52.499-0700","msg":"Get dynamic config","name":"system.archivalStatus","value":"","default-value":"","logging-call-at":"config.go:58"}
    {"level":"info","ts":"2019-06-19T14:16:52.499-0700","msg":"Get dynamic config","name":"system.enableReadFromArchival","value":"false","default-value":"false","logging-call-at":"config.go:58"}
    ...
    ...
    ...
    {"level":"info","ts":"2019-06-19T14:17:22.735-0700","msg":"none","service":"cadence-matching","component":"matching-engine","lifecycle":"Started","wf-task-list-name":"cadence-sys-tl-scanner-tasklist-0","wf-task-list-type":0,"logging-call-at":"matchingEngine.go:200"}
    {"level":"info","ts":"2019-06-19T14:17:52.517-0700","msg":"Get dynamic config","name":"history.timerProcessorCompleteTimerFailureRetryCount","value":"10","default-value":"10","logging-call-at":"config.go:58"}
    {"level":"info","ts":"2019-06-19T14:17:52.517-0700","msg":"Get dynamic config","name":"history.transferProcessorCompleteTransferFailureRetryCount","value":"10","default-value":"10","logging-call-at":"config.go:58"}
    {"level":"info","ts":"2019-06-19T14:22:22.738-0700","msg":"Get dynamic config","name":"matching.maxTasklistIdleTime","value":"5m0s","default-value":"5m0s","logging-call-at":"config.go:58"}
    
    ```



#### Run Cadence server with dockers

- **TO-DO**

### A Cadence Client Demo(Java)

The cadence-java-samples link[cadence-java-samples](https://github.com/uber/cadence-java-samples) works well.

First start the service:
```
cd $GOPATH/src/github.com/uber/cadence
./cadence-server start
```

When using ./gradlew build, some test cases didn't build successfully. Need to look into them.

### Java Hello Sample

[https://cadenceworkflow.io/docs/04_javaclient/01_workflow_interface](https://cadenceworkflow.io/docs/04_javaclient/01_workflow_interface)


**Activities and Workflows**
- Activities: the business logic units, interfaces and their implementations.
- Workflow: the flow or sequences of activities, a virtual object contains state, processing, threads.


#### Start Cadence Server
Configure your Cadence server and start it.

```bash
cd $GOPATH/src/github.com/uber/cadence
./cadence-server start
```

#### Register DOMAIN
```bash
./gradlew -q execute -PmainClass=com.uber.cadence.samples.common.RegisterDomain
```

#### Activity Interface and Implementation

Activity interface methods are annotated with @ActivityMethod, and will be implemented in activity implementations.

```java
public interface HelloOneTimeActivity {

  @ActivityMethod(scheduleToCloseTimeoutSeconds = 2)
  String composeGreeting(String greeting, String name);
}
```

```java
public class HelloOneTimeActivityImpl implements HelloOneTimeActivity {
  @Override
  public String composeGreeting(String greeting, String name) {
    return greeting + ", " + name + "!";
  }
}
```

#### Workflow Interface and Implementation

Workflow interface methods are annotated with @WorkflowMethod, and will be @override in the workflow implementations.

```java
public interface HelloOneTimeWorkflow {

  @WorkflowMethod(executionStartToCloseTimeoutSeconds = 10, taskList = HelloOneTimeWorker.TASK_LIST)
  String getGreeting(String name);
}
```

Within workflow implementation, use activity stub to invoke activities.  

```java
/** HelloOneTimeWorkflow implementation that calls HelloOneTimeActivity#composeGreeting. */
public class HelloOneTimeWorkflowImpl implements HelloOneTimeWorkflow {

  
  /**
   * Activity stub implements activity interface and proxies calls to it to Cadence activity
   * invocations. Because activities are reentrant, only a single stub can be used for multiple
   * activity invocations.
   */
  private static final HelloOneTimeActivity activities =
      Workflow.newActivityStub(HelloOneTimeActivity.class);

  @Override
  public String getGreeting(String name) {
    return activities.composeGreeting("Hello", name);
  }
}
```
#### Workers

Start a worker that hosts both workflow and activity implementations. Workflows are stateful, and activities are stateless and thread safe.

```java
public class HelloOneTimeWorker {
  public static final String TASK_LIST = "HelloOneTime";

  public static void main(String[] args) {
    Worker.Factory factory = new Worker.Factory(DOMAIN);
    Worker worker = factory.newWorker(TASK_LIST);
    worker.registerWorkflowImplementationTypes(HelloOneTimeWorkflowImpl.class);
    worker.registerActivitiesImplementations(new HelloOneTimeActivityImpl());
    factory.start();
    System.out.println("Worker started for task list: " + TASK_LIST);
  }
}
```


#### Clients/Starter

Start a workflow execution.

```java
public class HelloOneTimeStarter {

  public static void main(String[] args) {

    WorkflowClient client = WorkflowClient.newInstance(DOMAIN);

    HelloOneTimeWorkflow workflow = client.newWorkflowStub(HelloOneTimeWorkflow.class);

    String greeting = workflow.getGreeting("World");
    System.out.println(greeting);
    System.out.println("Workflow completed!");
    System.exit(0);
  }
}
```

#### Run examples

```bash
./gradlew -q execute -PmainClass=com.uber.cadence.samples.helloworldOneTime.HelloOneTimeWorker
```
```bash
./gradlew -q execute -PmainClass=com.uber.cadence.samples.helloworldOneTime.HelloOneTimeStarter
```

#### More examples

- Activity Retry
- Asynchronous activity
- Child Workflow
- Periodic Workflow
- Singals
- etc.

#### References
[https://cadenceworkflow.io/](https://cadenceworkflow.io/)

[https://github.com/uber/cadence](https://github.com/uber/cadence)

[https://github.com/uber/cadence-java-samples](https://github.com/uber/cadence-java-samples)

[https://www.youtube.com/watch?v=llmsBGKOuWI&t=3s](https://www.youtube.com/watch?v=llmsBGKOuWI&t=3s)