name: Build and Deploy

on:
  workflow_dispatch: {}

env:
  applicationfolder: spring-boot-hello-world-example
  AWS_REGION: ##region##
  S3BUCKET: ##s3-bucket## 


jobs:
  build:
    name: Build and Package
    runs-on: ubuntu-20.04
    permissions:
      id-token: write
      contents: read
    steps:
      - uses: actions/checkout@v2
        name: Checkout Repository

      - uses: aws-actions/configure-aws-credentials@v1
        with:
          role-to-assume: ${{ secrets.IAMROLE_GITHUB }}
          role-session-name: GitHub-Action-Role
          aws-region: ${{ env.AWS_REGION }}

      - name: Set up JDK 1.8
        uses: actions/setup-java@v1
        with:
          java-version: 1.8

      - name: chmod
        run: chmod -R +x ./.github

      - name: Build and Package Maven
        id: package
        working-directory: ${{ env.applicationfolder }}
        run: $GITHUB_WORKSPACE/.github/scripts/build.sh

      - name: Upload Artifact to s3
        working-directory: ${{ env.applicationfolder }}/target
        run: aws s3 cp *.war s3://${{ env.S3BUCKET }}/
        
  deploy:
    needs: build
    runs-on: ubuntu-latest
    environment: Dev
    permissions:
      id-token: write
      contents: read
    steps:
    - uses: actions/checkout@v2
    - uses: aws-actions/configure-aws-credentials@v1
      with:
        role-to-assume: ${{ secrets.IAMROLE_GITHUB }}
        role-session-name: GitHub-Action-Role
        aws-region: ${{ env.AWS_REGION }}
    - run: |
        echo "Deploying branch ${{ env.GITHUB_REF }} to ${{ github.event.inputs.environment }}"
        commit_hash=`git rev-parse HEAD`
        aws deploy create-deployment --application-name CodeDeployAppNameWithASG --deployment-group-name CodeDeployGroupName --github-location repository=$GITHUB_REPOSITORY,commitId=$commit_hash --ignore-application-stop-failures
