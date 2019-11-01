# e-identification-test-sp
 Shibboleth SP from Docker HUB https://hub.docker.com/r/unicon/shibboleth-sp

##Run at AWS

  Load shibboleth keys and certs (sp-encrypt-cert.pem  sp-encrypt-key.pem  sp-signing-cert.pem  sp-signing-key.pem)   
  to AWS Secretsmanager : https://eu-west-1.console.aws.amazon.com/secretsmanager/home?region=eu-west-1#/secret?name=e-identification-testsp-keys  
  List AWSCURRENT version of e-identification-testsp-keys   
  aws-vault exec vrk-tunnistus-dev -- aws secretsmanager list-secrets

  ```json
  aws-vault exec vrk-tunnistus-dev -- aws secretsmanager list-secrets
  ...output
        {
            "Name": "e-identification-testsp-keys", 
            "Tags": [], 
            "LastChangedDate": 1566295288.591, 
            "SecretVersionsToStages": {
                "f9cd7d66-4c7f-4126-83d2-aa3cbf606b5a": [
                    "AWSPREVIOUS"
                ], 
                "2b38e21f-3d06-4c40-a920-95f3fc403083": [
                    "AWSCURRENT"
                ]
            }, 
            "LastAccessedDate": 1566259200.0, 
            "ARN": "arn:aws:secretsmanager:eu-west-1:585109340096:secret:e-identification-testsp-keys-n4cqRg"
        }
  ```
  
  Update AWSCURRENT version of key secret to secrets.yml and apply the configuration.

  ```console
  kubectl.qa apply -f kubernetes/secrets.yaml 
  ```

  Build and push  
  Prerequisites, the repo for new image is created at AWS  
  https://eu-west-1.console.aws.amazon.com/ecr/repositories?region=eu-west-1
  You may need to docker login to csc and aws.
  For AWS you may need execute "aws-vault exec vrk-tunnistus-dev -- aws ecr get-login --no-include-email" for docker login  
  
  ```console
  cd /e-identification-docker-images/e-identification-test-sp  
  docker login e-identification-docker-virtual.vrk-artifactory-01.eden.csc.fi  
  export target_registry=585109340096.dkr.ecr.eu-west-1.amazonaws.com  
  export registry=e-identification-docker-virtual.vrk-artifactory-01.eden.csc.fi  

  docker build -f Dockerfile -t ${registry}/e-identification-test-sp:${oma_tagi} .  
  docker push ${registry}/e-identification-test-sp:${oma_tagi}  

  docker tag ${registry}/e-identification-test-sp:${oma_tagi} ${target_registry}/e-identification-test-sp:${oma_tagi}
  docker push ${target_registry}/e-identification-test-sp:${oma_tagi}  
   ```
  Create service and ingress (only the first time)  
  Update your oma_tagi to deployment.yaml and deploy  
  Check the ingress and browse to the service  
  
```console
  kubectl.qa apply -f kubernetes/service.yaml  
  kubectl.qa apply -f kubernetes/ingress.yaml  
  kubectl.qa apply -f kubernetes/deployment.yaml  
  kubectl.qa get ingress
```

  Hostname is oidc-test-sp.apro.tunnistus.fi    
  Test the service at   
  https://oidc-test-sp.apro.tunnistus.fi/Shibboleth.sso/Session  
  https://oidc-test-sp.apro.tunnistus.fi/Shibboleth.sso/Status    
     
##Run at localhost
Copy shibboleth keys and certs (sp-encrypt-cert.pem  sp-encrypt-key.pem  sp-signing-cert.pem  sp-signing-key.pem)   
from AWS Secretsmanager to your local machine /data/secrets  

```console
 cd /e-identification-docker-images/e-identification-test-sp  
 docker build --tag "e-identification-test-sp" .  

 cat '127.1.1.1 oidc-test-sp.apro.tunnistus.fi' > /etc/hosts  
 docker run -d -p 443:443 --name="test-sp" -v /data/secrets:/data00/secrets e-identification-test-sp:latest    
```
 