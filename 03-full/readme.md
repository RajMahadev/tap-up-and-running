# indicates that all packages that are part of tap.tanzu.vmware.com should be installed 
profile: full

# indicates that the customer acknwoldges that the CEIP data collection 
# program has been explained to them. 
ceip_policy_disclosed: true # Installation fails if this is set to 'false'

# Configure contour / envoy with an external loadbalancer whos
# ip you can add to DNS so that you can access the TAP domains 
# You will need a wild card subdoman for example *.tap.lotusinfo.tech
#
# TAP has a gui that the rest of this file will configure to deplay
# on tap-gui.tap.lotusinfo.tech 
#
# TAP has learning center that run on demand workshops. We will 
# configure Tanzu Learning center to deploy to 
# *.tlc.tap.lotusinfo.tech
#
# The Cloud Native Runtime is going to need a domain name to
# deploy knative services onto. We will configure this domain
# to be *.cnr.tap.lotusinfo.tech
#
# To be able to configure all these domain you need to configure
# contour deployment to use an externa LoadBalancer
contour:
  envoy:
    service:
      type: LoadBalancer

# Setup the Cloud Native Runtime Settings 
# Deploy and app called foo into namespace bar 
# 
# foo-bar.cnr.tap.lotusinfo.tech 
# 
# demoapp in namespace test 
# 
# demoapp-test.cnr.tap.lotusinfo.tech
cnrs:
  domain_name: cnr.tap.lotusinfo.tech
  domain_template: "{{.Name}}-{{.Namespace}}.{{.Domain}}"

# learning center
learningcenter:
  ingressDomain: "tlc.tap.lotusinfo.tech"

# tap gui
tap_gui:
  service_type: ClusterIP
  ingressEnabled: "true"
  ingressDomain: "tap.lotusinfo.tech"
  app_config:
    auth:
      environment: development
      providers:
        github:
          development:
            clientId: "80b9a7b2439976279278"  # you need to get this value from GitHub Developer Settings OAuth2 page
            clientSecret: "839802350c57e808e6f665047f637ef3df3c207f"
    app:
      baseUrl: http://tap-gui.tap.lotusinfo.tech
    catalog:
      locations:
        - type: url
          target: https://github.com/asaikali/tap-gui-sample-catalog/blob/main/catalog-info.yaml
    backend:
      baseUrl: http://tap-gui.tap.lotusinfo.tech
      cors:
        origin: http://tap-gui.tap.lotusinfo.tech

# setup the build service 
buildservice:
  kp_default_repository: "rajatos,azurecr.io/build-service"  # Build service images will be stored here, not application images containerzed by TBS
  kp_default_repository_username: "rajatos" 
  kp_default_repository_password: "gRjZWo+ffjJ8ekLrmQwvVUps9cj8UGRH"
  tanzunet_username: "graj@onemagnify.com" # creds for tanzu net so that installer can pull images from tanzu net and put them in the defalut repo 
  tanzunet_password: "Sup3rS0n1c$"
  descriptor_name: "tap-1.0.0-full"
  enable_automatic_dependency_updates: true


# setup supply chain 
supply_chain: basic
# supply_chain: testing
#supply_chain: testing_scanning

# ootb_supply_chain_testing:

#ootb_supply_chain_testing_scanning:
ootb_supply_chain_basic:
  registry:
    server: "rajatos,azurecr.io" # TBS will write container images to this OCI registry 
    repository: "apps"  # contairezed app images will go into this repo 
  gitops:
    ssh_secret: ""

  grype:
  #namespace: "MY-DEV-NAMESPACE" # (optional) Defaults to default namespace.
  targetImagePullSecret: "registry-credentials"
