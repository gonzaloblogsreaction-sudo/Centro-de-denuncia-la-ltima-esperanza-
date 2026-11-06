name: Crear APK Android

on:
  push:
    branches: [main]
  workflow_dispatch:

permissions:
  contents: read

jobs:
  build:
    runs-on: ubuntu-latest

    steps:
      - name: Descargar proyecto
        uses: actions/checkout@v4

      - name: Preparar Java
        uses: actions/setup-java@v4
        with:
          distribution: temurin
          java-version: '17'

      - name: Preparar Android
        uses: android-actions/setup-android@v3

      - name: Instalar Android SDK
        run: sdkmanager "platforms;android-35" "build-tools;35.0.0"

      - name: Preparar Gradle
        uses: gradle/actions/setup-gradle@v4
        with:
          gradle-version: '8.9'

      - name: Descomprimir aplicación
        run: unzip CentroBienestarAnimal-proyecto.zip

      - name: Construir APK
        working-directory: CentroBienestarAnimal
        run: gradle assembleDebug

      - name: Guardar APK
        uses: actions/upload-artifact@v4
        with:
          name: CentroBienestarAnimal-APK
          path: CentroBienestarAnimal/app/build/outputs/apk/debug/app-debug.apk
