# teraform-_with_linode_cloud


terraform {
  required_providers {
    linode = {
        source = "linode/linode"
    }
  }
}

provider "linode" {
    token       = "b3ce0bae96f2ab33abf9d0e93083c708e7b60746bf21061dbb679742c5d5673f"
    api_version = "v4beta" 
  
}

# Linode
resource "linode_instance" "example_instance" {
    label       = "example_instance_label"
    image       = "linode/ubuntu18.04"
    region      = "us-southeast"
    type        = "g6-standard-1"
    root_pass   = "vdoxx999" 

  
}
#domain 
resource "linode_domain" "example_domain" {
    domain      = "virtualdoxxindia.com"
    soa_email   = "apoorvcreate@gmail.com"
    type        = "master"
}
# domain record
resource "linode_domain_record" "example_domain_record" {
    domain_id =  linode_domain.example_domain.id
    name = "virtualdoxxindia.com"
    record_type = "A"
    target = linode_instance.example_instance.ip_address
    ttl_sec = 300
  
}

# firewall
resource "linode_firewall" "example_firewall" {
    label = "example_firewall_label"

  inbound {  
    label       = "allow-http"
    action      = "ACCEPT"
    protocol    = "TCP"
    ports       = "80"
    ipv4        = ["0.0.0.0/0"]
    ipv6        = ["ff00::/8"]  
  }
  inbound_policy = "DROP"

  outbound_policy = "ACCEPT"

  linodes = [linode_instance.example_instance.id]

}



  

   

