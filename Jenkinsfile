pipeline {
    agent {
        node {
            label 'pasq'
            
            //Files to scan with types included
            //files= [['AWS_S3_VersioningConfiguration.json','CFT'],[ 'main.tf','tf012'],['sockshop.yaml','k8s']]
               
                   //withCredentials([usernamePassword(credentialsId: 'prisma-cloud-accesskey', passwordVariable: 'PC_PASS', usernameVariable: 'PC_USER')]) {
                //PC_TOKEN = sh(script:"curl -s -k -H 'Content-Type: application/json' -H 'accept: application/json' --data '{\"username\":\"$PC_USER\", \"password\":\"$PC_PASS\"}' https://${AppStack}/login | jq --raw-output .token", returnStdout:true).trim()
                //}
            }
    }
    environment {
        PRISMAAUTH = credentials('prisma_cloud')
        PC_CONSOLE = 'console-master-pasq3.prussiello.demo.twistlock.com'
        //AppStack = 'https://api.prismacloud.io'
        
    }
    stages {
        stage('Clone repository') {
            steps {
                checkout scm
            }
        }

        //$PC_USER,$PC_PASS,$PC_CONSOLE
        stage('Download latest twistcli') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'prisma_cloud', passwordVariable: 'PC_PASS', usernameVariable: 'PC_USER')]) {
                sh 'curl -k -u $PC_USER:$PC_PASS --output ./twistcli https://$PC_CONSOLE/api/v1/util/twistcli'
                sh 'sudo chmod a+x ./twistcli'
            }
            }
        }
        stage ('get files'){
            steps {
                //files = [ 'main.tf','variables.tfvars','variables.tf']
                withCredentials([usernamePassword(credentialsId: 'prisma_cloud', passwordVariable: 'PC_PASS', usernameVariable: 'PC_USER')]) {
                    PC_TOKEN = sh(script:"curl -k -H 'Content-Type: application/json' -H 'accept: application/json' --data '{\"username\":\"$PC_USER\", \"password\":\"$PC_PASS\"}' https://api.prismacloud.io/login | jq --raw-output .token", returnStdout:true).trim()
                }
            }
        }

//         files.each { item ->
//             stage("Scan IaC file ${item[0]} with twistcli") {
//                 steps {
//                 catchError(buildResult: 'SUCCESS', stageResult: 'FAILURE') {
//                     withCredentials([usernamePassword(credentialsId: 'prisma-cloud-accesskey', passwordVariable: 'PC_PASS', usernameVariable: 'PC_USER')]) {
//                         sh "./twistcli iac scan -u $PC_USER -p $PC_PASS --asset-name Jenkins-IaC --tags env:jenkins  --address https://$PC_CONSOLE --type ${item[2]} example/${item[0]}"
//                         //sh "./twistcli iac scan -u $PC_USER -p $PC_PASS --asset-name Jenkins-IaC --tags env:jenkins --compliance-threshold high --address https://$PC_CONSOLE --type ${item[2]} files/${item[0]}"
//                         //sh "./twistcli iac scan --u $PC_USER --p $PC_PASS --compliance-threshold high --address https://$PC_CONSOLE files/${item}"
//                     }
//                 }
//                 }
//             }
//         }

//         //files.each { item ->
//            stage("Scan with Rest API v2 - ${item[2]}") {
//                steps {
//                     //Define a Scan
//                     def response
//                     response = sh(script:"curl -sq -X POST -H 'x-redlock-auth: ${PC_TOKEN}' -H 'Content-Type: application/vnd.api+json' --url https://${AppStack}/iac/v2/scans --data-binary '@scan-asset.json'", returnStdout:true).trim()

//                     def SCAN_ASSET = readJSON text: response

//                     //Save the ScanId and ScanURL
//                     //The ScanURL is the s3 location to upload 
//                     def SCAN_ID = SCAN_ASSET['data'].id
//                     def SCAN_URL = SCAN_ASSET['data']['links'].url

//                     //Upload files
//                     sh(script:"curl -sq -X PUT  --url '${SCAN_URL}' -T 'files/${item[0]}'", returnStdout:true).trim()

//                     //start the Scan
//                     response = sh(script:"curl -sq -X POST -H 'x-redlock-auth: ${PC_TOKEN}' -H 'Content-Type: application/vnd.api+json' --url https://${AppStack}/iac/v2/scans/${SCAN_ID} --data-binary '@${item[1]}'", returnStdout:true).trim()


//                     //Get the Status
//                     def SCAN_STATUS
//                     def STATUS

//                     //Need a Do-While loop here.   Haven't found a good syntax with Groovy in Jenkins
//                     response = sh(script:"curl -sq -H 'x-redlock-auth: ${PC_TOKEN}' -H 'Content-Type: application/vnd.api+json' --url https://${AppStack}/iac/v2/scans/${SCAN_ID}/status", returnStdout:true).trim()
//                     SCAN_STATUS = readJSON text: response
//                     STATUS = SCAN_STATUS['data']['attributes']['status']

//                     while  (STATUS == 'processsing'){
//                            response = sh(script:"curl -sq -H 'x-redlock-auth: ${PC_TOKEN}' -H 'Content-Type: application/vnd.api+json' --url https://${AppStack}/iac/v2/scans/${SCAN_ID}/status", returnStdout:true).trim()
//                            SCAN_STATUS = readJSON text: response
//                            STATUS = SCAN_STATUS['data']['attributes']['status']
//                            print "${STATUS}"
//                     }

//                     //Get the Results
//                     response = sh(script:"curl -sq -H 'x-redlock-auth: ${PC_TOKEN}' -H 'Content-Type: application/vnd.api+json' --url https://${AppStack}/iac/v2/scans/${SCAN_ID}/results", returnStdout:true).trim()
//                     def SCAN_RESULTS= readJSON text: response
//                         print "${SCAN_RESULTS}"
//                     }
//            }
//         }
    }
}
