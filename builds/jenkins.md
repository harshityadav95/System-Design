# Jenkins

### Jenkins Configs &#x20;

#### 1. Sample Pipeline&#x20;

* Type pipeline &#x20;
* Build Perodically : \* \* \* \* \*  5 stars for every minute run&#x20;
* Pipeline Script &#x20;

```
node {
    timestamps{
        stage "code build"
        echo "Stage 1 done"
        
        stage "Testing"
        echo "Testing Completed"
        
        stage "Final"
        echo "sucess"
    }
}
```

#### 2. Dummy Pipeline SCM

* Type : Pipeline&#x20;
* Pipeline Scripts from SCm&#x20;
* Source  :Git &#x20;
* Source Containing File named Jenkins &#x20;

```
node{
  timestamps{
  
  stage "Code Build"
  echo "Stage 1 Done"
  
  stage "Testing"
  echo "Testing Completed"
  
  stage "Final"
  echo "Sucess"
    
  }
}
```

#### 3 . FreeStyle &#x20;

* Type : Freestyle
* Source : Git repository added&#x20;
* Build  : Execute Window batch command &#x20;

```
Dir
```

#### 4. MVN Maven Pipeline Projects

* Install the Maven Build if not have already
* Type  : Maven &#x20;
* Source : Github Repo &#x20;
* Build Environment  :
  * Delete workspace before build starts
  * Add timestamp to console output
* Pre Steps  : Execute windows batch command &#x20;

```
echo " Maven Build Started"
```

* Build  : pom.xml from repo of the maven project
* Goal and Option : clean test
* Post Steps  : Run Regardless of build result

#### 5. Unit Testing&#x20;

* &#x20;Type  : Maven&#x20;
* Repo : Github
* Maven : clean test
*   Post Build Action  : &#x20;

    * Install NUnit plugin in jenkin
    * Publish NUnit test result report



#### 6. JMeter

* Install JMeter as plugin in the maven
* &#x20;Type  : Maven&#x20;
* Repo : Github
* Maven : clean test
* Build  : Execute the windows batch command as both are on the same OS (Jenkins + Jmeter)

```
(Jmeter config command to use the console version of the Jmeter)
```

*   Post Build Action  : &#x20;

    * Publish Performance test result report&#x20;



#### 7. TestNg + Maven Project

* type : Maven
* Repository  : GIt&#x20;
* Environment  : Delete workspace before build starts
* Build  : clean
* Post Build : Publish Test Ng results &#x20;

#### 8. Parametrized Project

* Freestyle
* Parameter : Java Parameter

#### 9. Multi Configuration&#x20;

* Type : MultiConfiguration&#x20;
* Source  : Git&#x20;
*







