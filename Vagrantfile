Vagrant.configure("2") do |config|
  # 테라폼을 실행할 베이스 리눅스 이미지 지정
  config.vm.box = "bento/ubuntu-24.04"
  config.vm.box_architecture = "arm64"
  # 가상머신 네트워크 설정 (로컬 PC에서 접근 가능하도록)
  config.vm.network "private_network", ip: "192.168.56.10"

  # 가상머신 스펙 설정 (메모리 2GB, CPU 2코어)
  config.vm.provider "virtualbox" do |vb|
    vb.memory = "2048"
    vb.cpus = 2
  end

  # 가상머신 부팅 시 Terraform 자동 설치
  config.vm.provision "shell", inline: <<-SHELL
    set -e
    export DEBIAN_FRONTEND=noninteractive

    echo "Installing Terraform..."
    sudo apt-get update
    sudo apt-get install -y gnupg software-properties-common curl wget

    wget -O- https://apt.releases.hashicorp.com/gpg | \
      sudo gpg --dearmor -o /usr/share/keyrings/hashicorp-archive-keyring.gpg

    echo "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/hashicorp-archive-keyring.gpg] https://apt.releases.hashicorp.com $(lsb_release -cs) main" | \
      sudo tee /etc/apt/sources.list.d/hashicorp.list

    sudo apt-get update
    sudo apt-get install -y terraform

    echo "Terraform version:"
    terraform --version
  SHELL
end 