pipeline {
    agent any

    // ⚠️ 注意：为了测试原生多分支或 Scan 插件，这里故意不写 triggers {} 块
    // 如果你必须用 Generic Webhook Trigger，请在 UI 界面配置，不要写在这里

    stages {
        stage('🔍 分支信息检测') {
            steps {
                script {
                    echo "=================================================="
                    echo "🚀 流水线已触发！正在输出调试信息..."
                    echo "=================================================="
                    
                    // 1. 输出当前任务所属的分支 (这是 Jenkins 自动识别的)
                    echo "📌 当前运行的分支 (BRANCH_NAME):  【 ${env.BRANCH_NAME} 】"
                    
                    // 2. 输出当前的 Commit ID
                    echo "🔗 当前代码 Commit ID:           ${env.GIT_COMMIT}"
                    
                    echo "--------------------------------------------------"

                    // 3. (可选) 如果你是用 Generic Webhook Trigger 触发的
                    // 并且配置了提取变量 ref，这里可以帮你对比
                    if (env.ref) {
                        echo "📨 Webhook 接收到的 ref 参数:    ${env.ref}"
                        
                        // 简单的逻辑判断，帮你看清楚为什么会触发
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
