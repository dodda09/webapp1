#!/usr/bin/env groovy

//Main pipeline excute command..

println 'printing env.JOB_NAME'
println env.JOB_NAME

println 'printing env.BRANCH_NAME'
println env.BRANCH_NAME

println 'Calling execute ...'
execute ('webapp')




/*
* 
*  Create a pipeline
*
* @parm 
*   projectName Is required parmeter and is the name of the project.
*
*/

def execute(String projectName)
{

    println 'in execute starting..'
    println projectName

    node 
    {
        try
        {
            //Check Out SCM
            checkout scm
            println 'checkout done'

            if (projectName == "webapp") 
            {
 
                println 'in if block'

                //Create a Pipeline
                if(env.BRANCH_NAME.contains("UI") || env.BRANCH_NAME == "master")
                    this.createWebAppPipeline()
                //else 
                //    throw new GradleException("Branch name invalid: " + env.BRANCH_NAME)

            } else if ( projectName == "pacfile") 
            {

                 println 'in else block'

                //Create Pipeline
                this.createPACFilePipeline()
            }
        }
        catch (Exception e)
        {
            currentBuild.result = 'FAILURE'

            throw e;
        }

    }
}


def createWebAppPipeline()
{
    //Build , Sonar , Artifactory , Deploy to DEV

    println 'in createWebAppPipeline'
    println env.BRANCH_NAME

    if(env.BRANCH_NAME.contains("UI"))
    {
        println 'in createWebAppPipeline UI'

        this.stageBuild()
        this.stageSonarqube()

    } else if (env.BRANCH_NAME == "master")
    {

        println 'in createWebAppPipeline master'

        this.stageBuild()
        this.stageSonarqube()
        this.stageInstallToNexus()
        this.stageDevDeploy()
    }
}


def createPACFilePipeline()
{
    //Build , Sonar , Artifactory , Deploy to DEV, Integartion Tests

    println 'in createPACFilePipeline'
    println env.BRANCH_NAME

    if(env.BRANCH_NAME.contains("PAC"))
    {
        println 'in createPACFilePipeline PAC'

        this.stageBuild()
        this.stageSonarqube()

    } else if (env.BRANCH_NAME == "master")
    {
        println 'in createPACFilePipeline master'

        this.stageBuild()
        this.stageSonarqube()
        this.stageInstallToNexus()
        this.stageDevDeploy()
        this.stageRunIntergationTest()
    }
}

/**
* Build 
**/
def stageBuild()
{
    stage('Build Code') {


        echo 'Building....'
    }
}

/*
* Run Sonar coverage
*/
def stageSonarqube()
{
    stage('SonarQube Checks') {
        echo 'running sonar quality gate checks..'
    }
}


/**
* Install to Artifactory
*/
def stageInstallToNexus()
{
    stage('Install To Nexus') {
        echo 'Installing to artifactory'
    }
}


/**
* Deploy to DEV
*/
def stageDevDeploy()
{
    stage('Deploy to DEV') {
        echo 'Deploying to DEV'
    }   
}


/*
* Run Interagtion Tests
*/
def stageRunIntergationTest()
{
    stage('Run IntergationTests') {
        echo 'Runing Integration Tests..'
    }
}