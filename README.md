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
