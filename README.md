# SecretsManagerDemo

## GitHub Runner & Docker Setup
- Setup a new Github runner by creating a new repository -> Settings -> Actions -> Runners -> New self-hosted runner -> Linux
  - Follow the provided step-by-step instructions to deploy the runner on a remote Linux server
  - You will need to install docker on the Linux server, which can be achieved by following the steps in the link below:
    - https://docs.cyberark.com/secrets-manager-sh/13.8/en/content/integrations/github-actions.htm

## Create the YAML File
- Within the new repository create the new directory path to the new file (e.g. ".github/workflows/deploy.yml")
  - Update the "url" with the domain of the tenant you're using (e.g. "https://<Tenant_Domain>.secretsmgr.cyberark.cloud/api")
    - Note: this specifically targets the Secrets Manger API
  - Update the "authn_id" with whatever works best for you (e.g. "github-jwt-chad-production")
  - Update the "secrets" with whatever value you are trying to pull back (i.e. account username, password, etc.)
    - For password, you need to pipe "CONJUR_PASS"
    - For username, you need to pipe "CONJUR_USERNAME"
    - These are the environment variables that the values will be bound to.
    - If you need to retrieve both of these values in the same instance, the must be separated with a semicolon.
- Start the runner by executing the following script:
  - "./run.sh" on the Linux server.

## Secrets Manager Setup
- Create a new authenticator (e.g. "JSON Web Tokens (JWT)")
  - Select the workload type (e.g. "GitHub Actions")
  - Specify a name (e.g. "github-jwt")
  - Provide the JWKS URI (e.g. "https://token.actions.githubusercontent.com/.well-known/jwks")
    - For more info refer to the following link: https://docs.github.com/en/actions/reference/security/oidc
  - Provide the Issure URL (e.g. "https://token.actions.githubusercontent.com")
    - For more info refer to the following link: https://docs.github.com/en/actions/reference/security/oidc
  - Under JWT claims mapping, specify the following:
    - Identity path with "data"
    - Token app property with "repository"
    
- Create a new workload (i.e. Workloads -> Create workload -> GitHub Actions)
  - The name of your workload should resemble the following format:
    - <GitHub_Username>/<GitHub_Repository_Name>
    - e.g. "ChadPapineau/SecretsManagerDemo"
  - For the branch location, select "data" then "Next"
  - Select the Authentication Service ID that you previously created (i.e. "github-jwt")
  - For the key-value pair, keep "repository" as the key and set the value to the same name you used for the workload (e.g. "ChadPapineau/SecretsManagerDemo")
  - Specify a safe
    
    
