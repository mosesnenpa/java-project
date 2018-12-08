node('linux') {
    stage('Unit Tests') {
        git 'https://github.com/mosesnenpa/java-project.git'
        sh 'ant -f test.xml -v'
        junit 'reports/*.xml'
    }
     stage('Build') {
        sh 'ant -f build.xml -v'
    } 
    
    withCredentials([[$class: 'AmazonWebServicesCredentialsBinding', accessKeyVariable: 'AWS_ACCESS_KEY_ID', credentialsId: 'e8c1d0d5-1353-40ca-a5b4-f61a7ff3fd55', secretKeyVariable: 'AWS_SECRET_ACCESS_KEY']]) {
        stage('Deploy') {
    		sh 'aws s3 cp ${WORKSPACE}/*/rectangle-${BUILD_NUMBER}.jar s3://hw10-1/rectangle-${BUILD_NUMBER}.jar'
    	}
    }
    withCredentials([[$class: 'AmazonWebServicesCredentialsBinding', accessKeyVariable: 'AWS_ACCESS_KEY_ID', credentialsId: 'e8c1d0d5-1353-40ca-a5b4-f61a7ff3fd55', secretKeyVariable: 'AWS_SECRET_ACCESS_KEY']]) {
    	stage('Report') {
    	    sh 'aws cloudformation describe-stack-resources --region us-east-1 --stack-name jenkins-stack'
    	}
    }
}
