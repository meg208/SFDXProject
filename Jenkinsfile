#!groovy 

import groovy.json.JsonSlurperClassic 

  

node { 

     
     
    def BUILD_NUMBER=env.BUILD_NUMBER 

    def RUN_ARTIFACT_DIR="tests/${BUILD_NUMBER}" 

  

    def HUB_ORG='megha@brillio.com' 
    def loginUrl='https://login.salesforce.com'

    def CONNECTED_APP_CONSUMER_KEY='3MVG9G9pzCUSkzZtu4L5kLBDOyrHvM_kMwIzHAVN093iRgReC_4hj6dlYoyEinjrA1ntu_hd834F2kBu4EWRY' 

  

    def SFDC_HOST='https://login.salesforce.com' 

    def JWT_KEY_CRED_ID='70cb8ff6-118c-418e-bc4a-c655d6aee691' 

    def scratchOrg = 'MyDev' 

  

    println "Accessing code from git branch:...." 

    stage('Checkout Source Code') { 

        // when running in multi-branch job, one must issue this command 

        checkout scm 

    } 

    println "Build is in progress in org: ${scratchOrg}...." 

    withCredentials([file(credentialsId: JWT_KEY_CRED_ID, variable: 'jwt_key_file')]) { 

        stage('DevHub Authentication') { 

            println "DevHub authentication is in progress......." 

            rc = bat returnStatus: true, script: "sfdx force:auth:jwt:grant --clientid ${CONNECTED_APP_CONSUMER_KEY} --username ${HUB_ORG} --jwtkeyfile \"${jwt_key_file}\" --setdefaultdevhubusername --instanceurl ${SFDC_HOST}" 

            if (rc != 0) { error 'DevHub Org authorization failed....10' } 

            println rc 

        } 

         

        /* 
        stage('Create Scratch org') { 
            println "Scratch org ${scratchOrg} creation is in progress......." 
            rc = bat returnStatus: true, script: "sfdx force:org:create -s -f config/project-scratch-def.json -a ${scratchOrg}" 
            if (rc != 0) { error 'SFDX Command failed....20' } 
            println rc 
  
            rc = bat returnStatus: true, script: "sfdx force:user:password:generate --targetusername ${scratchOrg}" 
            if (rc != 0) { error 'SFDX Command failed....21' } 
            println rc 
        }*/ 

        stage('Deploy Code') { 

            println "Code deployment into Scratch org ${scratchOrg} is in progress......." 

            rc = bat returnStatus: true, script: "sfdx force:source:push -u ${scratchOrg}" 

            if (rc != 0) { error 'SFDX Command failed....30' } 

            println rc 

        } 

        /* 
        stage('Assign Permission to user') { 
            println "Assign Permission to use into Scratch org ${scratchOrg} is in progress......." 
            rc = bat returnStatus: true, script: "sfdx force:user:permset:assign -n Compare_two_Users -u ${scratchOrg}" 
            if (rc != 0) { error 'SFDX Command failed....35' } 
            println rc 
        } 
        stage('Importing Seed-Data') { 
            println "Importing Seed-Data into Scratch org ${scratchOrg} is in progress......." 
            rc = bat returnStatus: true, script: "sfdx force:data:tree:import --plan ./Seed_Data/export-demo-Account-Contact-plan.json -u ${scratchOrg}" 
            if (rc != 0) { error 'SFDX Command failed....40' } 
            println rc 
  
        } 
        stage('Code Review Automation') { 
            println 60 
  
        } 
        stage('QA Automation') { 
            dir("qaAutomation"){ 
                bat 'mvn clean install' 
            } 
            println 60 
  
        } 
        stage('Send Email Notification') { 
            /*         
            def subject = "JENKINS-NOTIFICATION: : Job '${env.JOB_NAME} [${env.BUILD_NUMBER}]'"  
  
            emailext( 
                mimeType: 'text/html', 
                replyTo: '$DEFAULT_REPLYTO', 
                subject: subject, 
                from: 'megha.parmar@brillio.com', 
                to: 'megha.parmar@brillio.com', 
                body: '${SCRIPT,template="email.template"}', 
                attachLog: true, 
                compressLog: true, 
                recipientProviders: [[$class: 'DevelopersRecipientProvider']] 
            ) 
            */ 

        } 

  

    } 

 
