name: Build APK

on:
  push:
    branches:
      - main
      - master
  pull_request:
    branches:
      - main
      - master

jobs:
  build:
    runs-on: ubuntu-latest
    
    steps:
      - name: Checkout code
        uses: actions/checkout@v4
      
      - name: Set up JDK 17
        uses: actions/setup-java@v4
        with:
          java-version: '17'
          distribution: 'temurin'
          cache: gradle
      
      - name: Set up Android SDK
        uses: android-actions/setup-android@v3
      
      - name: Set up Gradle
        uses: gradle/gradle-build-action@v2
      
      - name: Build APK
        run: gradle assembleDebug
      
      - name: Upload APK Artifact
        uses: actions/upload-artifact@v4
        with:
          name: JOS-Firewall-debug-apk
          path: app/build/outputs/apk/debug/app-debug.apk
