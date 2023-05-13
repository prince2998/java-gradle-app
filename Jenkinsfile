pipeline{
    agent any
    stages{
        stage("Git Test"){
            steps{
                echo "Executing git connection test"
            }
            post{
                success{
                    echo "git repository cloned successfully"
                }
                failure{
                    echo "git clone action failed"
                }
            }
        }
    }
    post{
        success{
            echo "========pipeline executed successfully ========"
        }
        failure{
            echo "========pipeline execution failed========"
        }
    }
}