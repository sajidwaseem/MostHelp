#!groovy

node {
    def linkProductRepo = true
    def repoBase = 'https://github.com/Guite/'
    def projectName = 'MostHelp'
    def repoUrl = repoBase + projectName + '/'
    def downstreamJobs = ['MOST-5 Deploy-Help', 'MOST-1_Prepare-9_Locales']
    def artifacts = '**/releng/**/target/repository/**,**/bundles/de.guite.modulestudio.help/doc/**,**/bundles/de.guite.modulestudio.help/pdfmanual/*.pdf'

    def builder, postProcessor
    stage('Init') {
        fileLoader.withGit("${repoBase}MostProduct.git", 'master', 'c568f590-e3fe-4732-9e5c-68ebc55b849e') {
            builder = fileLoader.load('vars/builder')
            postProcessor = fileLoader.load('vars/postBuild')
        }
    }

    try {
        builder.init(projectName, repoUrl, linkProductRepo)

        postProcessor.finish(repoUrl, artifacts, downstreamJobs);
    } catch (exception) {
        builder.handleError(repoUrl, exception)
        throw exception
    } finally {
        if (null != postProcessor) {
            postProcessor.finalise()
        }
    }
}
