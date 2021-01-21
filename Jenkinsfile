#!groovy
// vim: ft=groovy
// These 2 need to be authorized using jenkins script approval
// http://your.jenkins.host/scriptApprovald/
import groovy.json.JsonOutput
import java.util.Optional
import java.util.LinkedHashMap

////////////////////////////////////////
// MODIFY YOUR PROJECT VARIABLES
////////////////////////////////////////
@Library('workflowlibs') _
globalHelper = new globaldevops.helper.helper()

/********** User defined variables ****/
uuaa = "erul"							// example: qnos
countryRepoPrefix = "bgrt" // example: bgba for Spain
repoName = "loanrequest"					// example: BPMS_QNOS_QNOS
component = "process"						 // example: process
architectureVersion = "1.0.0" // example: 1.0.0 - Only applys to process component
dockerImageVersion = "0.0.5"				 // example: 0.0.1 - Only applys to process component
////////////////////////////////////////
// DO NOT MODIFY FURTHER THIS POINT
////////////////////////////////////////

/********** Environment variables **********/
node ('bpm'){
  globalrepo = "${env.globalrepo}"
  globalrepo_credentials_id = "${env.globalrepo_credentials_id}"
  projVerRepo = "${env.proj_ver_repo}"
  projVerRepoBucket = "${env.proj_ver_repo_bucket}"
  jenkinsProjVerCredentials = "${env.jenkins_proj_ver_credentials}"
  jenkinsSshCredentials = "${env.jenkins_ssh_credentials}"
  jenkinsArtifactoryCredentials = "${env.jenkins_artifactory_credentials}"
  artifactoryServer = "${env.artifactory_server}"
  workspace = "${env.WORKSPACE}"
}

/********** Global variables **********/
globalrepoBranch = "develop"
bpm_node = "bpm"

/********** Project variables **********/
def repoParams = [
        "uuaa" : uuaa,
        "countryRepoPrefix": countryRepoPrefix,
        "component" : component,
        "repoName" : repoName,
		"architectureVersion" : architectureVersion,
        "dockVer" : dockerImageVersion,
        "repoBranch" : "${env.BRANCH_NAME}",
        "workspace" : "${workspace}"
]

/************ CI Flow ************/

stage("Checkout Global Library") {
  fileLoader.withGit(globalrepo, globalrepoBranch, globalrepo_credentials_id, bpm_node) {
    bpm_ci = fileLoader.load('src/bpm/ci/ci');
  }
}

bpm_ci.bpm_ci_flow(repoParams)
