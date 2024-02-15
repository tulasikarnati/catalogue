#!groovy
@Library('roboshop-shared-library') _

// respinsibility is to pass application and component to pipeline decission
def configMap = [
    application: "nodejsVM",
    component: "catalogue"
]

if(! env.BRANCH_NAME == "main") {
    pipelineDecission.decidepipeline(configMap)
} else {
    echo "This is production, please go with CR process"
}