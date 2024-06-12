  provider "aws" {
  region = "ap-south-1"
}

resource "aws_vpc" "main" {
  cidr_block = "10.0.0.0/16"
}

resource "aws_subnet" "main" {
  vpc_id            = vpc-0c44efd049db947a8
  cidr_block        = "10.0.0.0/24"
  map_public_ip_on_launch = true
}

resource "aws_internet_gateway" "main" {
  vpc_id = vpc-0c44efd049db947a8
}

resource "aws_route_table" "main" {
  vpc_id = vpc-0c44efd049db947a8

  route {
    cidr_block = "0.0.0.0/0"
    gateway_id = igw-00c6e2934541b769b
  }
}

resource "aws_route_table_association" "main" {
  subnet_id      = subnet-0a632bda6691b6436
  route_table_id = rtb-0d221f66fa6cbdde7
}

resource "aws_security_group" "web_sg" {
  vpc_id = vpc-0c44efd049db947a8

  ingress {
    from_port   = 22
    to_port     = 22
    protocol    = "tcp"
    cidr_blocks = ["0.0.0.0/0"]
  }
ingress {
    from_port   = 80
    to_port     = 80
    protocol    = "tcp"
    cidr_blocks = ["0.0.0.0/0"]
  }

resource "aws_instance" "web" {
  ami           = "ami-085046f4dd2c36cd4"
  instance_type = "t2.micro"
  subnet_id     = subnet-0a632bda6691b6436
  security_groups = [launch-wizard-4 TERRAFORM 2]

  user_data = file("../setup/install_flask.sh")

  tags = {
    Name = "FlaskWebServer"
  }
}
