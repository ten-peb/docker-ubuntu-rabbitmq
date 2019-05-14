// -=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-
//
// Jenkinsfile defining a mini pipeline to build and stage a Docker image
// containing the following:
//    RabbitMQ
//    Puppet Agent
//    Nagios NRPE server
//
// Author
//     Peter L. Berghold <pberghold@tenna.com>
//
// Copyright and License
//
//     This work is copyrighted (c) Copyright 2019, Tenna LLC all rights
//     reserved.
//
//     No derived works from this should be made without consulting the
//     author or a representative of Tenna LLC and without express permission.
//
// -=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-
node("master"){

  def String giturl = "git@github.com:ten-peb/docker-ubuntu-rabbitmq.git"
  def String clone2 = 'docker-rabbitmq'
  def String image_base_tag = "ubuntu-rabbitmq" 
  def String image_version_tag = "0.5.0"

  // Grab the latest version from GitHub to "clone2" subdirectory to work on
  stage("checkout source"){
    doGitClone(giturl,clone2)
  }
  // Build the image
  stage("build image"){
    dir(clone2){
      def String image_tag = image_base_tag + ':' + image_version_tag;
      doDockerBuild(image_tag)

      doDockerBuild(image_base_tag + ':latest')
    }		 
  }
  // Brag about it.
  stage("Send Notification of Success"){
    def String[] message = [
    "Greetings,",
    "This is to inform you that the Docker image ${image_base_tag} version ${image_version_tag}",
    "This image contains RabbitMQ and",
    "the Puppet agent and Nagios NRPE server",
    "This was successfully built and pushed to the registry.",
    " ",
    "Sincerely,",
    "Your faithful servant."," Jenkins"
    ]
    sendEmail(qaTeam(),"ubuntu-rabbitmq built",message.join("\n"))
  }
}