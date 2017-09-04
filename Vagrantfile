# -*- mode: ruby -*-
# vi: set ft=ruby :
Vagrant.configure("2") do |config|
  config.vm.define "bamboo-server" do |ba|
    # TODO volume mounting does not work - throws kahadb/log not found errors.
    # ba.vm.synced_folder "#{ENV['HOMEDRIVE']}/#{ENV['HOMEPATH']}/bamboo-docker-volume/",
    #   "/var/atlassian/bamboo",
    #   create: true
    ba.vm.provider :docker do |d|
      #d.image = "matisq/bamboo-server"
      d.build_dir = "./dockerfiles/bamboo"
      d.name = "bamboo-server"
      d.volumes = ["bamboo-server:/var/atlassian/bamboo"]
      d.ports = ["8085:8085","54663:54663"]
    end
  end

  config.vm.define "jira-server" do |j|
    j.vm.provider :docker do |d|
      d.build_dir = "./dockerfiles/jira/"
      d.name = "jira-server"
      d.ports = ["9090:8080"]
      # jira seems to require at least 2 GB memory
      d.create_args = ["--memory=2048m"]
      d.volumes = ["jira-server:/var/atlassian/jira"]
    end
  end

  config.vm.define "bitbucket-server" do |bb|
    # bb.vm.synced_folder "#{ENV['HOMEDRIVE']}/#{ENV['HOMEPATH']}/bitbucket-docker-volume/",
    #   "/var/atlassian/application-data/bitbucket",
      # create: true
    bb.vm.provider :docker do |d|
      d.image = "atlassian/bitbucket-server"
      d.name = "bitbucket-server"
      d.ports = ["7990:7990","7999:7999"]
      # docs recommend 2GB for bitbucket
      d.create_args = ["--memory=2048m"]
      d.volumes = ["bitbucket-server:/var/atlassian/application-data/bitbucket"]
    end
  end
  config.vm.define "tomcat-petclinic-dev" do |tc1|
    tc1.vm.provider :docker do |d|
      d.build_dir = "./dockerfiles/tomcat/"
      d.name = "tomcat-petclinic-dev"
      d.ports = ["8090:8080"]
    end
  end

  config.vm.define "tomcat-petclinic-qa" do |tc1|
    tc1.vm.provider :docker do |d|
      d.build_dir = "./dockerfiles/tomcat/"
      d.name = "tomcat-petclinic-qa"
      d.ports = ["8100:8080"]
    end
  end

end
