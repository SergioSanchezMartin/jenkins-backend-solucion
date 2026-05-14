pipeline {
  // Ejecuta sobre el nodo de Jenkins directamente
  agent any

  // 1. Opciones del job
    // - Deshabilitar builds concurrentes.
    //  - Mostrar marcas de tiempo.
    //  - Timeout de 5 minutos.
  options {
    disableConcurrentBuilds()
    timestamps()
    timeout(5)
  }
  // 2. Variables de entorno
  environment {
    FORCE_COLOR = "0"
    NO_COLOR = "true"
  }

  stages {
    // 3. Auditoría de herramientas
    stage('Audit tools') {
      steps {
        sh 'node --version'
      }
    }

    // 4. Instalación de dependencias
    stage('Install dependencies') {
      steps {
        sh 'npm install'
      }
    }

    // 5. Chequeo de formato de código
    stage('Format check') {
      steps {
        sh 'npm run format:check'
      }
    }

    // 6. Chequeo de calidad de código
    stage('Code quality') {
      steps {
        sh 'npm run lint'
      }
    }

    // 7. Chequeo de tipos
    stage('Type check') {
      steps {
        sh 'npm run type-check'
      }
    }

    // 8. Ejecución de tests
    stage('Tests') {
      steps {
        sh 'npm run test'
      }
    }

    // 9. Construcción y archivado
    stage('Build') {
      steps {
        sh 'npm run test'
        archiveArtifacts artifacts: 'dist/', fingerprint: true, followSymlinks: false
      }
    }
  }

  post {
    success {
      echo 'Pipeline completed successfully'
    }
    failure {
      // Con sh podemos interpolar variables de entorno con comillas simples
      sh 'echo "Pipeline failed. Review logs."'
    }
    always {
      cleanWs()
    }
  }


}
