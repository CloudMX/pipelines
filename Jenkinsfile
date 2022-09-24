pipeline {
    agent any
    stages {
        stage('Creando un archivo de propiedades') {
            steps {
                sh '''
                cat << EOF > dev.yaml
                project: "" # Project ID donde vive el cluster
                services:
                  gke:
                    # Requeridas:
                    cluster: "" # Nombre del cluster donde mandaremos el deployment
                    appName: "" # Nombre de la aplicacion
                    # No obligatorio
                    clusterLocation: "us-east1-b" # Zona del cluster
                    buildImage: false # Automaticamente lanza el job de crear la imagen
                    substitutions:
                      - var: VAR_NAME
                        value: VALUE
                        files:
                          - "path/"  
                EOF
                '''.stripIndent()
            }
        }

        stage('Mostrando contenido de archivo yaml'){
            steps {
                sh 'cat dev.yaml'
               }
        }


        stage('Usando metodo readYaml') {
            steps {
              script {
                def configVal = readYaml (file: "dev.yaml")
                echo "configVal: " + configVal
                env.BRANCH_NAME = configVal['project'][4,5,6]
                env.APP_PROJECT = configVal['project']
                env.APP_CLUSTER = configVal['services']['gke']['cluster']
                env.APP_NAME = configVal['services']['gke']['appName']
                env.APP_CLUSTER_LOCATION = configVal['services']['gke']['clusterLocation']
                env.APP_BUILD_IMAGE = configVal['services']['gke']['buildImage']
                env.APP_SUBSTITUTIONS = configVal['services']['gke']['substitutions']
                // Variables de entorno 
                echo env.BRANCH_NAME
                echo env.APP_PROJECT
                echo env.APP_CLUSTER
                echo env.APP_NAME
                echo env.APP_CLUSTER_LOCATION
                echo env.APP_BUILD_IMAGE
                echo env.APP_SUBSTITUTIONS
              }               
            }
        }
    }                
}
