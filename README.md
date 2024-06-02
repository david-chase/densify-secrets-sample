# Densify Secrets Sample

Many customers don't want to store their forwarder credentials in plain text in a configmap.  This can be easily worked around by using Kubernetes secrets.  These sample files will demonstrate how to deploy the Densify data forwarder v4 using Kubernetes secrets.  

Note that by default Kubernetes secrets are Base64 encoded which are neither secure nor encrypted.  However customers can integrate with a 3rd party secrets store like HashiCorp Vault to do so.  Please see the following for more information on integrating Kubernetes secrets with external secrets stores:

https://secrets-store-csi-driver.sigs.k8s.io/concepts.html#provider-for-the-secrets-store-csi-driver

## Step 1 - Deploy credentials as a secret

You will need to encode your forwarder user name and password (or epassword) in Base64 to proceed.  To do so, from a shell type:

  echo -n 'insert username' | base64
  echo -n 'insert password' | base64

Now edit densify-credentials.yaml and paste in your encoded credentials.  Deploy the secret the same way you would any other YAML file:

  kubectl apply -f densify-credentials.yaml

## Step 2 - Deploy the configmap

Notice that the sample configmap has the credentials commented out as they're not needed.  But be sure to add your instance name, port, and prometheus server and port before proceeding.

  kubectl apply -f configmap.yaml

## Step 3 - Deploy the pod

  kubectl apply -f pod.yaml

Have a look at the changes to pod.yaml and cronjob.yaml to see how the secrets are being used.  The customer will need to make similar changes to their YAML files before deploying the forwarder.  Keep in mind that these examples were made to use with a DaaS instance where you're using a password for the forwarder, not an epassword.  When using an epassword simply modify the pod.yaml and cronjob.yaml to change DENSIFY_PASSWORD to DENSIFY_EPASSWORD.

Have fun!
