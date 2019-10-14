#!/usr/bin/env groovy

properties([
  parameters([
    string(name: 'GIT_BRANCH', defaultValue: 'snippets', description: 'Git branch')
  ])
])

node('master') {
  def repo = 'feedback'
  def gitCommit
  def cmd = 'go run ./snippets/snippets.go'
  stage('Checkout') {
    git(
      credentialsId: 'github',
      url: "https://github.com/ynotgnef/${repo}.git",
      branch: "${params.GIT_BRANCH}"
    )
    gitCommit = sh(
      script: "git log --pretty=format:'%h' -n 1",
      returnStdout: true
    )
  }
  stage('Build Image') {
    sh "sudo docker build -t ${repo}:${gitCommit} ."
  }
  stage('Run Image') {
    sh """
      sudo docker container prune
      sudo docker run --name ${repo} ${repo}:${gitCommit} ${cmd}
    """
  }
  stage('Artifact Results') {
    echo 'nyi'
  }
}