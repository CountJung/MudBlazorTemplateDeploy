# MudBlazorTemplate
# Every Blazor Deployment Blogs Leaks Some important Information !!!🤬🤬🤬
### This is Quick Guide for deploy Blazor Web assembly to github Pages

1. Install Template of MudBlazor
follow this link 
https://github.com/MudBlazor/Templates

2. Create MudBlazor Template with wasm
 - and assume create github repository

3. Github Repository Setting
 - go to Settings -> Action -> General -> Workflow Permission
 ![ActionWorkflow](https://user-images.githubusercontent.com/47770079/225545607-b356b3b0-fd8f-4fdf-8e07-b96dde745ee3.png)

4. Setup Workflow file
```
name: Deploy to GitHub Pages
# Run workflow on every push to the master branch
on:
  push:
    branches: [ master ]
    
jobs:
  deploy-to-github-pages:
    # use ubuntu-latest image to run steps on
    runs-on: ubuntu-latest
    steps:
      # uses GitHub's checkout action to checkout code form the master branch
      - uses: actions/checkout@v2
      # sets up .NET Core SDK
      - name: Setup .NET Core SDK
        uses: actions/setup-dotnet@v1
        with:
          dotnet-version: 6.x
          include-prerelease: true
          
      # Install dotnet wasm buildtools workload
      - name: Install .NET WASM Build Tools
        run: dotnet workload install wasm-tools
      
      #- name: copy index.html to 404.html
      #  run: cp release/wwwroot/index.html release/wwwroot/404.html

      #- name: Add .nojekyll file
      #  run: touch release/wwwroot/.nojekyll
        
      # publishes Blazor project to the release-folder
      - name: Publish .NET Core Project
        run: dotnet publish MudBlazorTemplate/MudBlazorTemplate.csproj -c Release -p:GHPages=true -o release --nologo
      
      - name: Commit wwwroot to GitHub Pages
        uses: JamesIves/github-pages-deploy-action@3.7.1
        with:
          GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}
          BRANCH: gh-pages
          FOLDER: release/wwwroot
```
 * note: I created 404 and .nojekyll file for convenience
 
5. After Push Your Blazor Code
 - go to Settings -> Pages -> Build and Deployment
 - Source = Deploy from branch
 - Branch = gh-pages
 ![githubPageBuildAndDeploy](https://user-images.githubusercontent.com/47770079/225546403-1f19882c-1b93-4b2b-aa20-c76f75fe3195.png)

6. make sure what I have lost, missed setting
![setting1](https://user-images.githubusercontent.com/47770079/225549296-85b8bf7c-3c22-4cd6-9a9a-1c9d4c478db3.png)

7. reference
https://swimburger.net/blog/dotnet/how-to-deploy-aspnet-blazor-webassembly-to-github-pages
https://github.com/mammadkoma/MySite
