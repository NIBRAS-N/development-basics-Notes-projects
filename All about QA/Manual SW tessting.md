# part-1

### What is Software? Types of Software’s?

- Software refers to a collection of instructions, programs, data, and documentation that perform specific tasks on a computer or electronic device.
- #### `Types of SW`:
  - `System Software:`
    - Operating Systems :  Examples include Windows, macOS, Linux, Android, and iOS.
      
    - Device Drivers: Software that enables communication between hardware devices and the operating system.
      
    - Utility Programs: Tools that perform maintenance, optimization, and troubleshooting tasks, such as antivirus software, disk defragmenters, and backup utilities.
   
    - Servers

  - `Application Software`:
 
    - Application software refers to programs designed to perform specific tasks or functions for end-users.
   
    - EG: `web app , mobile app , desktop app`
   
  - `Programming SW`:
      - **Compilers**:
          - Compilers are software tools that translate high-level programming languages (such as C, C++, Java) into machine code or intermediate code that can be executed by a computer's processor.
          - They analyze the entire source code of a program and generate an executable file or another form of output (such as bytecode for Java).
      - **debuggers**
          - Debuggers are software tools used by developers to identify and resolve errors, bugs, and other issues in a program.
      - **Interpeters**
          - Interpreters are software tools that execute high-level programming languages directly, without the need for prior compilation into machine code.
          - They read and interpret source code line by line and execute it sequentially.

### What is Software Testing?

  - Software testing is a process of evaluating a software application or system to ensure that it meets specified requirements and functions correctly.

### What is SW quality?

  - Software quality refers to the degree to which a software product or system meets specified requirements and user expectations.
  - bug free
  - delivered on time
  - within budget
  - meets requirements or expectations
  - maintainable

###  Project Vs Product:

  - if software application is developed for a specific customer based on the requirement then it is called project.
  - if software application is developed for multiple customers based on market requirements then it is called product.

### Why do we need testing?

  - ensure the software is bug free
  - ensure the system meets customer requirements and software specification
  - ensure that system meets end user expectations

### ERROR : 
  - any incorrect human action that produces a problem in the system is called an error
### Defect/BUG:
  - deviation from the expected behavior to the actual behavior of the system is called defect
### Failure:
  - the deviation  identified by end user while using the system is called failure
    
### Why software have bug?

  - Miscommunication or no communication
  - Software complexity
  - Programming errors
  - Changing requirements

# Part-2:

### SDLC:
  - software development life cycle is a process used by software industry to design development test softwares
  - ![image](https://github.com/NIBRAS-N/development-basics-Notes-projects/assets/83491751/831fba81-79a6-4e2f-81f2-38c5835ee842)

### WaterFall Model:

  - The Waterfall model is a linear sequential software development lifecycle model that follows a strict step-by-step approach.
  - ![image](https://github.com/NIBRAS-N/development-basics-Notes-projects/assets/83491751/e6f4e462-006c-4815-90e7-b85c8783b1a4)
  - ![image](https://github.com/NIBRAS-N/development-basics-Notes-projects/assets/83491751/aa5a9d4a-2d76-4eee-8665-f28f709302dc)

### Spiral model:
  - ![image](https://github.com/NIBRAS-N/development-basics-Notes-projects/assets/83491751/1e3fc6d4-9104-4b2c-82ff-d1f047871f02)
  - ![image](https://github.com/NIBRAS-N/development-basics-Notes-projects/assets/83491751/7db85c9b-e747-4571-bb1f-4887deda5a2c)

### V model:

  - ![image](https://github.com/NIBRAS-N/development-basics-Notes-projects/assets/83491751/7ca39f90-0ab4-466d-8474-4a51f0c927e8)
  - ![image](https://github.com/NIBRAS-N/development-basics-Notes-projects/assets/83491751/bc9d7a01-0b8e-40ed-a688-40771e939c3d)

### Testing:
  - ![image](https://github.com/NIBRAS-N/development-basics-Notes-projects/assets/83491751/c934d909-d316-4f3b-8f6f-62114d57c1d5)

  - `Static Testing:`
      - Analysis of a program carried out without executing the program.
      - Done during verification process i.e before development
      - code is not testd in this phrase, only documentation is tested.
      - Process:
          - Walkthrough
          - Insepction
          - Informal review Unplaned and undcoument
          - Technical Review , fault process
  - `Dynamic Testing`:
      - **Black Box Tesing (Functinal Testing)**
          - System Testing
              - in this testing we can test whole application (complete / integrated software is tested)
              - done by tester
          - UAT Testing
              - a level of software Testing in which software is tested for user acceptance
              - UAT done at client location where software is actually used
              - <i>Alpha Testing :</i> done by tester in company in presence of customer
              - <i>Beta Testing :</i> done by customer to check software is ok , satisfy requirement
              - 
      - **White Box Testing (Non Functional ):**
        
          - <i>Unit Testing</i>
            - in unit testing individual component of software tested.
            - (done by developer by using sample input and observing its sample output)
              
          - <i>Integration Teting</i> 
              - in integration Testing individual units are combined and tested as group (developer)
              - (i) Top-down
              - (ii) Bottom-up
              - (iii) Sandwich
              - (iv) Big-Bang
           
    - **Functional testing:**
          - Check a piece of software is acting in accordance with pre-determined requirements.
          - is a type of black-box testing.
      
    - **Non Functional Testing :**
      
          - checking how many people can simultaneously check out of a shopping basket
          - Load testing – gradually increase the load
          - Stress testing – suddenly increase the load (Eg. Online filling form)
          - Volume testing – how much data handle
          - Reliability
          - The readiness of a system
          - Usability testing
          - Security Testing
              - Authentication
              - Authorization
          - Recovery Testing
   
    - **Compatibility**
        - Forword - upgrade version
        - Backword - downgrade version
        - Hardware - hardware support
        - Installation testing
        - Uninstallation
          
    - **Sanitatoin and garbage**
        - Bug
          
    - **Smoke testing :**
        - (Testing on newly released build → compulsary requirement )
        - It is first testing on newly released build

    - **Sanity Testing :**
        - ( Testing on newly released build → check Compulsory + optional )
    - ![image](https://github.com/NIBRAS-N/development-basics-Notes-projects/assets/83491751/c4500bc4-3ff6-4961-81b6-a242352fcde5)
    - ![image](https://github.com/NIBRAS-N/development-basics-Notes-projects/assets/83491751/42abbacf-44d6-4903-89ed-7b9198addfb6)

  -![image](https://github.com/NIBRAS-N/development-basics-Notes-projects/assets/83491751/ac0c5e51-f07f-4d0d-9efc-d683fe04d9b7)

  - ![image](https://github.com/NIBRAS-N/development-basics-Notes-projects/assets/83491751/6556488f-a149-4bb2-a45a-9dbc97bb2847)

  -![image](https://github.com/NIBRAS-N/development-basics-Notes-projects/assets/83491751/64d48cad-c557-4cc5-9de3-075bfa0658b8)
 
  - ![image](https://github.com/NIBRAS-N/development-basics-Notes-projects/assets/83491751/f979f6b9-73b0-43cc-83d7-88a3ee51d797)

  - ![image](https://github.com/NIBRAS-N/development-basics-Notes-projects/assets/83491751/4e670790-82fe-4bd9-b30a-c7a59a564bf4)

  - #### Regression testing:
    - It is overall testing when ever new change is occurred .
    - ![image](https://github.com/NIBRAS-N/development-basics-Notes-projects/assets/83491751/642f96b1-c5c0-4e11-be7c-f9c92a136bc7)
  
  - #### Re-testing:
    - ![image](https://github.com/NIBRAS-N/development-basics-Notes-projects/assets/83491751/0de8cd90-4631-4b97-9a27-26369e9ed5ff)
    
# part-3

###  QA:

    - quality assurance 
    - process related 
    - high level management
    - Process designed by QA 
    - Responsible for highest possible quality
    - It is process related
    - responsible for preventing defect
    - 
### QC:

    - Actual testing of software
    - quality control 
    - product related 
    - actual tester
    - responsible for finding defect
    
# part - 4:

## Test Case Design Technique:

    - It helps better design and reduce the number of test case to be executed
    - Reduce data and more coverage

  - Types:
      - 1. ECP Equivalence Class Partition
            - value check
            - classify | divide | partition data in → multiple classes to save testing time
        3. BVA Boundary value analysis
        4. Design Table
        5. State transition
        6. Error guessing
  
# part - 5:

- ### STLC: Software Testing Life Cycle:
    - ![image](https://github.com/NIBRAS-N/development-basics-Notes-projects/assets/83491751/d0ae1bec-d0ea-4f6d-b66a-e7ad0a891fc4)
    - ![image](https://github.com/NIBRAS-N/development-basics-Notes-projects/assets/83491751/8d8bf0df-9a09-4bb1-a6c6-983c34d216f7)

- ### Requirement Traceblity matrix : (RTM)

  - Trace how many Test case are execuated or covered
  - in simple keep track of test cases
  - ![image](https://github.com/NIBRAS-N/development-basics-Notes-projects/assets/83491751/9d8aaee3-255c-4dcb-8d10-fffa04ee0091)
