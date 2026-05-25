// @Library('jenkins-test-library') // Name declared in In Jenkins URL -> Settings -> System -> Global Trusted Pipeline Libraries -> Add

// def configMap =[
//     project: "roboshop",
//     component: "catalogue"
// ]
// echo "Triggering the Library pipeline"

// if(env.BRANCH_NAME.equalIgnoreCase('main')){
//     echo "executing main branch code"
// }
// else{
//     testpipeline(configMap) //groovy filename created in shared-library-test project
// }

def configMap = [
    project: "roboshop",
    component: "catalogue"
]

echo "Triggering the library pipeline"

if ( env.BRANCH_NAME.equalsIgnoreCase('main') ){
    configMap["jiraProject"] = "ROBO"
    EKSMainPipeline(configMap)
}
else{
    configMap["jiraProject"] = "ROBO"
    nodeJSEKSPipeline(configMap)
}