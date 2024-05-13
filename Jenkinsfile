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

        // Get the list of files in the classes directory
        def classesDir = 'force-app/main/default/classes'
        def classesFiles = findFiles(glob: "${classesDir}/**/*.cls").collect { it.path }

        // Output the files present in the classes directory
        echo "Files in ${classesDir}:"
        classesFiles.each { fileName ->
            echo fileName
        }
    
    // Extract class names from package.xml, excluding wildcard entries
        def packageClasses = packageXmlContent.readLines().findAll { line ->
            line.contains('<members>') && line.contains('</members>') && !line.contains('<members>*</members>')
        }.collect { line ->
            line.replaceAll(/<members>|<\/members>/, '').trim()
        }

        echo "Extracted class names from package.xml:"
        packageClasses.each { className ->
            echo className
        }

    // Convert class names and file names to lowercase for case-insensitive comparison
        def lowercasePackageClasses = packageClasses.collect { it.toLowerCase() }
        def lowercaseClassesFiles = classesFiles.collect { it.toLowerCase() }

    // Validate class names
        def missingClasses = packageClasses.findAll { className ->
            !classesFiles.any { it.endsWith("/${className}.cls") }
        }

        if (missingClasses) {
            echo "Missing classes in package.xml:"
            missingClasses.each { echo it }
            error "Validation failed: Some classes are missing in package.xml"
        } else {
            echo "All classes are present in package.xml"
        }
    }
}