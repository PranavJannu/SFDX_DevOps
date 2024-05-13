node {
    def BUILD_NUMBER=env.BUILD_NUMBER
    def RUN_ARTIFACT_DIR="tests/${BUILD_NUMBER}"
    def SFDC_USERNAME

    def HUB_ORG=env.HUB_ORG_DH
    def SFDC_HOST = env.SFDC_HOST_DH
    def JWT_KEY_CRED_ID = env.JWT_CRED_ID_DH
    def CONNECTED_APP_CONSUMER_KEY=env.CONNECTED_APP_CONSUMER_KEY_DH

    def toolbelt = tool 'toolbelt'

    stage('checkout source') {
        // when running in multi-branch job, one must issue this command
        checkout scm
        echo "Under checkout source stage"
        bat 'git ls-files'
    }

    stage('Read Files') {
        def packageXmlContent = readFile 'manifest/package.xml'
        echo "Package.xml content: ${packageXmlContent.trim()}" //Disply files present under package.xml
        echo "Reading package.xml file"

       def files = bat(
            script: "ls force-app/main/default/classes/",
            returnStdout: true
            ).trim().split('\n')
            for (def file in files) {
                def content = readFile(file)
                // Process content as needed
        }
    }
}
