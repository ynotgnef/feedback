#!/usr/bin/env groovy

properties([
  string(name: 'GIT_BRANCH', defaultValue: 'master', description: 'Git branch')
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
    sh "docker build -t ${repo}:${gitCommit} ."
  }
  stage('Run Image') {
    sh """
      docker container prune
      docker run --name ${repo} ${repo}:${gitCommit} ${cmd}
    """
  }
  stage('Artifact Results') {
    echo 'nyi'
  }
}