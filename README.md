# Create IAM admin user
- sign into AWS console as root
- create new IAM user with AWS specific admin permission
- save credential file
- logout and log back in as new user

# Install AWS Cli

# Install Hugo
brew install hugo

# Check Hugo version
hugo version

# Create new site
hugo new site voiceserverlesswebsite

# Install theme
- goto themes page: https://themes.gohugo.io/
- selected Learn theme
- follow theme installation instructions: https://github.com/matcornic/hugo-theme-learn
- open config.toml
- update theme to point to installed theme location theme="hugo-theme-learn"

# Change starting url
- open config.toml
- change baseURL to http://localhost:1313/ as this is the default port hugo serves on

# For multilingual themes, add Default
- open config.toml
- add defaultContentLanguage = "en"
- add after theme defaultContentLanguageInSubdir = true

# Add parameters
- open config.toml
- add
    [params]
        description = "This is a description of serverless website"
        author = "Author's name"
        showVisitedLinks = true

    [Languages]
    [Languages.en]
    title = "This is the english title"
    weight = 1
    languageName = "English"

    [Languages.fr]
    title = "Route d'excellence"
    weight = 2
    languageName = "Francais"

# Run site
-D option will show all draft when running site
hugo server -D

# Building site for serverless hosting
builds the site and resources and places into public folder
push the contents of this folder to s3 folder
- execute "hugo"
- add gitignore file so public folder is not accidentally published
touch .gitignore
voiceserverlesswebsite/public

# CircleCi
- mkdir .circleci
- touch config.yml
- create build def
- hugo installtion requires workaround
- touch hugo_installation_config.txt
- goto https://copr.fedorainfracloud.org/coprs/daftaupe/hugo/
- copy CentOS Epel content into new file
- push all to repo
- sign up in circleci site using github account
- add project
- select repo
- start building

# Create S3 Buckets
Create bucket for hosting the website
- aws s3 mb s3://voice-serverless-website-site
Create bucket for hosting the lambda code
- aws s3 mb s3://voice-serverless-website-code-bucket
Create bucket for hosting the language files processed by lambda
- aws s3 mb s3://voice-serverless-website-polly-bucket

# Create IAM user for CircleCi and S3 access
- goto IAM
- create new user (programmatic access)
- attach existing policies directly
- create new policy (s3, all permissions except delete, add restricted resources website and code buckets)
- finish creating user with new policy
- save the credentials file
- goto circle ci -> insights -> proj settings (gear icon) -> aws permssions and use the access and secret key for the user

# Update S3 site bucket to host static site
- make note of url
- update config.toml baseURL with s3 bucket url
- in config.toml, add uglyurls = true

# Create html files for each supported language
- hugo new --kind chapter _index.en.md
- hugo new --kind chapter _index.fr.md
- open the files and set draft = false

# Update circleci config to build and deploy
new run
- cd voiceserverlesswebsite
- hugo
new deploy
delete contents before syncing
set access to public read
encrypt using standard server side encryption
- aws s3 sync --delete --acl "public-read" --sse "AES256" voiceserverlesswebsite/public
commit changes to source control to trigger build