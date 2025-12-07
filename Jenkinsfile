pipeline {
    agent any
    stages {
        stage('🔍 分支信息检测') {
            steps {
                script {
                    echo "=================================================="
                    echo "🚀 流水线已触发！正在输出调试信息..."
                    echo "=================================================="
                    
                    echo "📌 当前运行的分支 (BRANCH_NAME):  【 ${env.BRANCH_NAME} 】"
                    echo "🔗 当前代码 Commit ID:           ${env.GIT_COMMIT}"
                    
                    echo "--------------------------------------------------"

                    if (env.ref) {
                        echo "📨 Webhook 接收到的 ref 参数:    ${env.ref}"
                        
                        def cleanRef = env.ref.minus('refs/heads/')
                        
                        if (cleanRef == env.BRANCH_NAME) {
                            echo "✅ 匹配成功：Webhook 指向的分支与当前任务分支一致。"
                        } else {
                            echo "❌ 匹配异常：Webhook 指向 [${cleanRef}]，但当前任务是 [${env.BRANCH_NAME}]。"
                            echo "   (如果你看到了这句话，说明你的过滤逻辑没生效，导致错误的流水线被触发了)"
                        }
                    } else {
                        echo "ℹ️ 未检测到 'ref' 变量 (可能是通过原生 Git/Scan 插件触发，或手动点击触发)"
                    }
                  
                    echo "=================================================="
                }
            }
        }
    }
}
