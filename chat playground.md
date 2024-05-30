*Estas diseñando un curso sobre el uso de la inteligencia artificial en las operaciones de integración continua y despliegue automatizado por medio de actions de Github. Quieres terminar el curso de 4 horas con una actividad que puedan hacer los alumnos que tienen cuentas en Github. Describe al detalle esta práctica y todos los pasos a realizar.*


## Título de la Actividad: Implementación de un Pipeline CI/CD con Análisis de Código Basado en IA en GitHub Actions
 

### Objetivo:
Los alumnos aprenderán a configurar un pipeline de CI/CD en GitHub Actions que incluye la integración de una herramienta de análisis de código basada en inteligencia artificial para mejorar la calidad del código y automatizar el despliegue.
### Requisitos Previos:
* Cuenta en GitHub.
* Conocimiento básico de GitHub Actions.
* Proyecto de ejemplo en un repositorio de GitHub (puede ser un simple proyecto en C# .NET).

## Pasos a Realizar:

#### Paso 1: Crear un Repositorio en GitHub
* Inicia sesión en tu cuenta de GitHub.
* Crea un nuevo repositorio. Puedes nombrarlo *ci-cd-ai-demo*.
* Inicializa el repositorio con un *README.md* y un archivo *.gitignore* adecuado para un proyecto en C#.

#### Paso 2: Configurar el Proyecto de Ejemplo
* Clona el repositorio en tu máquina local.
* Crea un nuevo proyecto en C# .NET (puede ser una aplicación de consola simple) y subelo al repositorio.

``` yaml
dotnet new console -o MyApp  
cd MyApp  
dotnet build  
dotnet run  
git add .  
git commit -m "Initial commit"  
git push origin main  
```

#### Paso 3: Configurar GitHub Actions para CI
* En el repositorio en GitHub, ve a la pestaña Actions.
* Haz clic en *Set up a workflow yourself* para crear un nuevo archivo de flujo de trabajo.
* Crea un archivo .github/workflows/ci.yml con el siguiente contenido:

```yaml
name: CI Pipeline  

on:  
  push:  
    branches: [ main ]  
  pull_request:  
    branches: [ main ]  

jobs:  
  build:  
    runs-on: ubuntu-latest  

    steps:  
      - name: Checkout code  
        uses: actions/checkout@v2  

      - name: Setup .NET Core  
        uses: actions/setup-dotnet@v2  
        with:  
          dotnet-version: '8.0.x'  

      - name: Restore dependencies  
        run: dotnet restore  

      - name: Build  
        run: dotnet build --no-restore  

      - name: Run tests  
        run: dotnet test --no-build --verbosity normal  
``` 

#### Paso 4: Integrar Herramienta de Análisis de Código Basada en IA
Vamos a usar [DeepSource](https://deepsource.com/), una herramienta de análisis de código con capacidades de IA. 
* Regístrate en [DeepSource](https://deepsource.com/) y enlaza tu repositorio.
* En tu repositorio, modifica el archivo *.deepsource.toml* con el siguiente contenido:

```yaml
version = 1  

[[analyzers]]  
name = "csharp"  
enabled = true  
```
* Actualiza el archivo ci.yml para incluir el análisis de código:


```yaml
name: CI Pipeline  

on:  
  push:  
    branches: [ main ]  
  pull_request:  
    branches: [ main ]  

jobs:  
  build:  
    runs-on: ubuntu-latest  

    steps:  
      - name: Checkout code  
        uses: actions/checkout@v2  

      - name: Setup .NET Core  
        uses: actions/setup-dotnet@v2  

      - name: Restore dependencies  
        run: dotnet restore  
  
      - name: Build  
        run: dotnet build --no-restore  
  
      - name: Run tests  
        run: dotnet test --no-build --verbosity normal  
  
      - name: Run DeepSource Analysis  
        run: curl https://deepsource.io/cli | sh  
          ./bin/deepsource report --analyzer csharp   
``` 
 

#### Paso 5: Configurar Despliegue Automatizado
Vamos a configurar el despliegue automatizado a un entorno de Azure. 
* Asegúrate de tener una cuenta de Azure y haber creado un App Service.
* En el portal de Azure, ve al App Service y selecciona *Deployment Center*. 
* Configura las credenciales de despliegue y obtén el nombre del usuario y el secreto de despliegue.
* En GitHub, ve a la configuración del repositorio y selecciona *Secrets*. 
* Agrega los siguientes secretos:

AZURE_WEBAPP_NAME: El nombre de tu App Service.
AZURE_WEBAPP_PUBLISH_PROFILE: El contenido del perfil de publicación (puedes obtenerlo desde Azure Portal).

Actualiza el archivo *ci.yml* para incluir el despliegue automatizado:

```yaml
name: CI/CD Pipeline  

on:  
  push:  
    branches: [ main ]  
  pull_request:  
    branches: [ main ]  

jobs:  
  build:  
    runs-on: ubuntu-latest  

    steps:  
      - name: Checkout code  
        uses: actions/checkout@v2  

      - name: Setup .NET Core  
        uses: actions/setup-dotnet@v2  
        with:  
          dotnet-version: '8.0.x'  

      - name: Restore dependencies  
        run: dotnet restore  

      - name: Build  
        run: dotnet build --no-restore  

      - name: Run tests  
        run: dotnet test --no-build --verbosity normal  

      - name: Run DeepSource analysis  
        run: docker run --rm -v $(pwd):/workspace deepsourceio/analyzer run --analyzer csharp  

      - name: Deploy to Azure  
        uses: azure/webapps-deploy@v2  
        with:  
          app-name: ${{ secrets.AZURE_WEBAPP_NAME }}  
          publish-profile: ${{ secrets.AZURE_WEBAPP_PUBLISH_PROFILE }}  
          package: .  
``` 

#### Paso 6: Validar la Configuración
Haz un commit y un push de todos los archivos de configuración al repositorio.

```yaml
git add .  
git commit -m "Add CI/CD pipeline with AI code analysis"  
git push origin main
```
 
* Ve a la pestaña *Actions* en tu repositorio en GitHub y verifica que el flujo de trabajo se haya ejecutado correctamente. Deberías ver las etapas de *build, test, análisis de código y despliegue*.

* Verifica en DeepSource que los análisis de código se estén ejecutando y proporcionando feedback sobre el código.

* Finalmente, verifica en Azure que tu aplicación se haya desplegado correctamente.

### Resumen de la Actividad:
* Crear un Repositorio en GitHub: Configurar un repositorio con un proyecto de ejemplo en C#.
* Configurar GitHub Actions para CI: Definir un flujo de trabajo para la construcción y prueba del código.
* Integrar Herramienta de Análisis de Código Basada en IA: Utilizar DeepSource para realizar análisis de código.

* Configurar Despliegue Automatizado: Configurar GitHub Actions para desplegar automáticamente la aplicación a un App Service de Azure.
* Validar la Configuración: Confirmar que todos los pasos del pipeline se ejecutan correctamente y que la aplicación se despliega exitosamente.

### Verificación Final:
#### Verificar GitHub Actions:
* Navega a la pestaña Actions en tu repositorio de GitHub.
* Asegúrate de que el flujo de trabajo se ha ejecutado correctamente sin errores.
* Revisa las etapas del flujo de trabajo: Checkout code, Setup .NET Core, Restore dependencies, Build, Run tests, Run DeepSource analysis, y Deploy to Azure.

#### Verificar DeepSource:
* Inicia sesión en DeepSource y navega al análisis de tu repositorio.
* Revisa los reportes generados y verifica que no haya errores críticos o vulnerabilidades en el código.
* Toma nota de las recomendaciones y sugerencias proporcionadas por DeepSource para mejorar la calidad del código.

### Verificar Despliegue en Azure:
* Inicia sesión en el portal de Azure y navega a tu App Service.
* Verifica que la última versión de tu aplicación esté desplegada y funcionando correctamente.
* Abre la URL de tu App Service para confirmar que la aplicación se ejecuta sin problemas.

### Consideraciones Adicionales:
* Notificaciones: Puedes agregar pasos adicionales en tu flujo de trabajo de GitHub Actions para enviar notificaciones a Slack, Teams, o correo electrónico cuando el despliegue sea exitoso o falle.
* Rollbacks: Implementa estrategias de rollback en caso de que una nueva versión de tu aplicación tenga errores críticos. Puedes configurar GitHub Actions para revertir a una versión anterior automáticamente.
* Monitorización y Alertas: Considera integrar herramientas de monitoreo y alertas, como Azure Application Insights, para supervisar el rendimiento y la disponibilidad de tu aplicación desplegada.

### Conclusión:
Al finalizar esta actividad, los alumnos habrán configurado un pipeline de CI/CD completo que incluye construcción, pruebas, análisis de código basado en inteligencia artificial y despliegue automatizado a un entorno de producción. 

Esta práctica les proporciona una comprensión sólida de cómo utilizar herramientas modernas para garantizar la calidad del código y automatizar los procesos de despliegue, lo cual es esencial en el desarrollo de software ágil y eficiente.




## Título de la Actividad: Implementación de un Pipeline CI/CD con Detección de Vulnerabilidades de Seguridad Basada en IA en GitHub Actions
 

Objetivo:
Los alumnos aprenderán a configurar un pipeline de CI/CD en GitHub Actions que incluye la integración de una herramienta de detección de vulnerabilidades de seguridad basada en inteligencia artificial, para garantizar la seguridad del código y automatizar el despliegue.
Requisitos Previos:
Cuenta en GitHub.
Conocimiento básico de GitHub Actions.
Proyecto de ejemplo en un repositorio de GitHub (puede ser un simple proyecto en C# .NET).
Pasos a Realizar:
 

Paso 1: Crear un Repositorio en GitHub
Inicia sesión en tu cuenta de GitHub.
Crea un nuevo repositorio. Puedes nombrarlo ci-cd-security-demo.
Inicializa el repositorio con un README.md y un archivo .gitignore adecuado para un proyecto en C#.
Paso 2: Configurar el Proyecto de Ejemplo
Clona el repositorio en tu máquina local.
Crea un nuevo proyecto en C# .NET (puede ser una aplicación de consola simple) y empújalo al repositorio.

dotnet new console -o MyApp  
cd MyApp  
dotnet build  
dotnet run  
git add .  
git commit -m "Initial commit"  
git push origin main  
 

Paso 3: Configurar GitHub Actions para CI
En el repositorio en GitHub, ve a la pestaña Actions.
Haz clic en "Set up a workflow yourself" para crear un nuevo archivo de flujo de trabajo.
Crea un archivo .github/workflows/ci.yml con el siguiente contenido:

name: CI Pipeline  

on:  
  push:  
    branches: [ main ]  
  pull_request:  
    branches: [ main ]  

jobs:  
  build:  
    runs-on: ubuntu-latest  

    steps:  
      - name: Checkout code  
        uses: actions/checkout@v2  

      - name: Setup .NET Core  
        uses: actions/setup-dotnet@v1  
        with:  
          dotnet-version: '5.0.x'  

      - name: Restore dependencies  
        run: dotnet restore  

      - name: Build  
        run: dotnet build --no-restore  

      - name: Run tests  
        run: dotnet test --no-build --verbosity normal  
 

Paso 4: Integrar Herramienta de Detección de Vulnerabilidades Basada en IA
Vamos a usar Snyk, una herramienta de detección de vulnerabilidades con capacidades de IA. Regístrate en Snyk y enlaza tu repositorio.
En tu repositorio, crea un archivo snyk.yml con el siguiente contenido:

version: 1.0.0  
projects:  
  - path: .  
 
3. Actualiza el archivo ci.yml para incluir el análisis de vulnerabilidades:


name: CI Pipeline  

on:  
  push:  
    branches: [ main ]  
  pull_request:  
    branches: [ main ]  

jobs:  
  build:  
    runs-on: ubuntu-latest  

    steps:  
      - name: Checkout code  
        uses: actions/checkout@v2  

      - name: Setup .NET Core  
        uses: actions/setup-dotnet@v1  
        with:  
          dotnet-version: '5.0.x'  

      - name: Restore dependencies  
        run: dotnet restore  

      - name: Build  
        run: dotnet build --no-restore  

      - name: Run tests  
        run: dotnet test --no-build --verbosity normal  

      - name: Run Snyk to check for vulnerabilities  
        uses: snyk/actions/setup@v1  
        with:  
          args: test  
        env:  
          SNYK_TOKEN: ${{ secrets.SNYK_TOKEN }}  
 

Paso 5: Configurar Despliegue Automatizado
Vamos a configurar el despliegue automatizado a un entorno de Azure. Asegúrate de tener una cuenta de Azure y haber creado un App Service.
En el portal de Azure, ve al App Service y selecciona "Deployment Center". Configura las credenciales de despliegue y obtén el nombre del usuario y el secreto de despliegue.
En GitHub, ve a la configuración del repositorio y selecciona "Secrets". Agrega los siguientes secretos:
AZURE_WEBAPP_NAME: El nombre de tu App Service.
AZURE_WEBAPP_PUBLISH_PROFILE: El contenido del perfil de publicación (puedes obtenerlo desde Azure Portal).
Actualiza el archivo ci.yml para incluir el despliegue automatizado:

name: CI/CD Pipeline  

on:  
  push:  
    branches: [ main ]  
  pull_request:  
    branches: [ main ]  

jobs:  
  build:  
    runs-on: ubuntu-latest  

    steps:  
      - name: Checkout code  
        uses: actions/checkout@v2  

      - name: Setup .NET Core  
        uses: actions/setup-dotnet@v1  
        with:  
          dotnet-version: '5.0.x'  

      - name: Restore dependencies  
        run: dotnet restore  

      - name: Build  
        run: dotnet build --no-restore  

      - name: Run tests  
        run: dotnet test --no-build --verbosity normal  

      - name: Run Snyk to check for vulnerabilities  
        uses: snyk/actions/setup@v1  
        with:  
          args: test  
        env:  
          SNYK_TOKEN: ${{ secrets.SNYK_TOKEN }}  

      - name: Deploy to Azure  
        uses: azure/webapps-deploy@v2  
        with:  
          app-name: ${{ secrets.AZURE_WEBAPP_NAME }}  
          publish-profile: ${{ secrets.AZURE_WEBAPP_PUBLISH_PROFILE }}  
          package: .  
 

Paso 6: Validar la Configuración
Haz un commit y un push de todos los archivos de configuración al repositorio.

git add .  
git commit -m "Add CI/CD pipeline with security vulnerability detection"  
git push origin main  
 
2. Ve a la pestaña Actions en tu repositorio en GitHub y verifica que el flujo de trabajo se haya ejecutado correctamente. Deberías ver las etapas de build, test, análisis de vulnerabilidades y despliegue.

3. Verifica en Snyk que los análisis de vulnerabilidades se estén ejecutando y proporcionando feedback sobre el código.

4. Finalmente, verifica en Azure que tu aplicación se haya desplegado correctamente.
Resumen de la Actividad:
Crear un Repositorio en GitHub: Configurar un repositorio con un proyecto de ejemplo en C#.
Configurar GitHub Actions para CI: Definir un flujo de trabajo para la construcción y prueba del código.
Integrar Herramienta de Detección de Vulnerabilidades Basada en IA: Utilizar Snyk para realizar análisis de vulnerabilidades de seguridad.
Configurar Despliegue Automatizado: Configurar GitHub Actions para desplegar automáticamente la aplicación a un App Service de Azure.
Validar la Configuración: Confirmar que todos los pasos del pipeline se ejecutan correctamente y que la aplicación se despliega exitosamente.
Verificación Final:
Verificar GitHub Actions:
Navega a la pestaña Actions en tu repositorio de GitHub.
Asegúrate de que el flujo de trabajo se ha ejecutado correctamente sin errores.
Revisa las etapas del flujo de trabajo: Checkout code, Setup .NET Core, Restore dependencies, Build, Run tests, Run Snyk to check for vulnerabilities, y Deploy to Azure.
Verificar Snyk:
Inicia sesión en Snyk y navega al análisis de tu repositorio.
Revisa los reportes generados y verifica que no haya vulnerabilidades críticas en el código.
Toma nota de las recomendaciones y sugerencias proporcionadas por Snyk para mejorar la seguridad del código.
Verificar Despliegue en Azure:
Inicia sesión en el portal de Azure y navega a tu App Service.
Verifica que la última versión de tu aplicación esté desplegada y funcionando correctamente.
Abre la URL de tu App Service para confirmar que la aplicación se ejecuta sin problemas.
Conclusión:
Al finalizar esta actividad, los alumnos habrán configurado un pipeline de CI/CD completo que incluye construcción, pruebas, análisis de vulnerabilidades de seguridad basado en inteligencia artificial y despliegue automatizado a un entorno de producción. Esta práctica les proporciona una comprensión sólida de cómo utilizar herramientas modernas para garantizar la seguridad del código y automatizar los procesos de despliegue, lo cual es esencial en el desarrollo de software seguro y eficiente.
Recursos Adicionales:
GitHub Actions Documentation
Snyk Documentation
Azure App Service Documentation
Feedback y Preguntas:
Al final de la actividad, abre un espacio para preguntas y feedback. Asegúrate de resolver cualquier duda que los alumnos puedan tener y proporcionarles recursos adicionales si es necesario.

Espero que esta segunda actividad sea igualmente útil para tu curso y que los alumnos encuentren valiosa la experiencia de implementar un pipeline CI/CD con detección de vulnerabilidades de seguridad basada en IA. ¡Buena suerte con el curso!

Tengo un action de Github que está devolviendo un error porque no encuentra ningún fichero de solución o de proyecto para hacer la tarea de Build. El proyecto está dentro de una carpeta MyApp ¿Qué tengo que cambiar en el steps de Build para que me encuentre el fichero de proyecto?

Para solucionar el problema de que GitHub Actions no encuentra el archivo de proyecto o solución, necesitas especificar la ruta correcta al archivo en el paso de Build. En tu caso, el proyecto está dentro de una carpeta llamada MyApp, por lo que debes cambiar el directorio de trabajo a MyApp antes de ejecutar el comando de construcción.

Aquí tienes cómo puedes modificar el archivo .github/workflows/ci.yml para que el paso de Build se ejecute correctamente:

name: CI Pipeline  
  
on:  
  push:  
    branches: [ main ]  
  pull_request:  
    branches: [ main ]  
  
jobs:  
  build:  
    runs-on: ubuntu-latest  
  
    steps:  
      - name: Checkout code  
        uses: actions/checkout@v2  
  
      - name: Setup .NET Core  
        uses: actions/setup-dotnet@v1  
        with:  
          dotnet-version: '5.0.x'  
  
      - name: Restore dependencies  
        run: dotnet restore MyApp/MyApp.csproj  
  
      - name: Build  
        run: dotnet build MyApp/MyApp.csproj --no-restore  
  
      - name: Run tests  
        run: dotnet test MyApp/MyApp.csproj --no-build --verbosity normal  
 

Explicación:
Checkout code: Este paso permanece igual y se utiliza para clonar el repositorio en el runner de GitHub Actions.
Setup .NET Core: Este paso también permanece igual y se utiliza para configurar el entorno de .NET Core.
Restore dependencies: Aquí se especifica la ruta al archivo del proyecto (MyApp/MyApp.csproj). Esto asegura que el comando dotnet restore se ejecute en el contexto correcto y restaure las dependencias del proyecto.
Build: Similar al paso anterior, se especifica la ruta al archivo del proyecto (MyApp/MyApp.csproj). Esto asegura que el comando dotnet build se ejecute en el contexto correcto.
Run tests: Nuevamente, se especifica la ruta al archivo del proyecto (MyApp/MyApp.csproj) para ejecutar las pruebas en el contexto correcto.
Alternativa Usando working-directory:
Otra forma de solucionar el problema es cambiar el directorio de trabajo para todos los comandos de .NET Core. Aquí tienes cómo hacerlo:

name: CI Pipeline  
  
on:  
  push:  
    branches: [ main ]  
  pull_request:  
    branches: [ main ]  
  
jobs:  
  build:  
    runs-on: ubuntu-latest  
  
    steps:  
      - name: Checkout code  
        uses: actions/checkout@v2  
  
      - name: Setup .NET Core  
        uses: actions/setup-dotnet@v1  
        with:  
          dotnet-version: '5.0.x'  
  
      - name: Restore dependencies  
        run: dotnet restore  
        working-directory: MyApp  
  
      - name: Build  
        run: dotnet build --no-restore  
        working-directory: MyApp  
  
      - name: Run tests  
        run: dotnet test --no-build --verbosity normal  
        working-directory: MyApp  
 

Explicación:
working-directory: Esta opción se utiliza para especificar el directorio de trabajo para cada comando. Al establecer working-directory: MyApp, cada comando dotnet se ejecutará dentro de la carpeta MyApp, donde se encuentra tu archivo de proyecto.

Ambas soluciones son válidas y aseguran que los comandos de .NET Core se ejecuten en el contexto correcto para encontrar los archivos de proyecto necesarios. Elige la opción que prefieras y actualiza tu archivo de flujo de trabajo en GitHub Actions.