pipeline {
    agent none
    
    triggers {
        GenericTrigger(
            genericVariables: [
                [key: 'ref', value: '$.ref'],
                [key: 'ref_name', value: '$.ref', regexpFilter: 'refs/heads/'] 
            ],
            genericRequestVariables: [
                [key: 'url_branch', regexpFilter: '']
            ],
            token: 'test-branch-name-token',
            
            // 🧪 测试方案 1：直接使用 ${BRANCH_NAME}
            regexpFilterText: '$ref',
            regexpFilterExpression: '^refs/heads/${BRANCH_NAME}$',
            
            printContributedVariables: true,
            printPostContent: true,
            causeString: 'Test: ref=$ref, ref_name=$ref_name, url_branch=$url_branch, BRANCH_NAME=${BRANCH_NAME}'
        )
    }
    
    stages {
        stage('🧪 测试报告') {
            agent any
            steps {
                script {
                    def report = """
========================================
🧪 TRIGGERS 阶段测试报告
========================================

📌 测试目标：
   验证 \${BRANCH_NAME} 在 triggers 块中是否可用

📊 测试数据：
   - 当前分支 (BRANCH_NAME): ${env.BRANCH_NAME}
   - GitLab 完整 ref: ${env.ref}
   - GitLab 分支名 (ref_name): ${env.ref_name}
   - URL 参数分支 (url_branch): ${env.url_branch}

🔍 触发原因：
   ${currentBuild.getBuildCauses()}

========================================
📋 测试结论：
========================================
"""
                    
                    if (env.ref_name == env.BRANCH_NAME) {
                        report += """
✅ 方案 1 成功：\${BRANCH_NAME} 在 triggers 中可用
   - 正则表达式正确匹配了当前分支
   - 只有 ${env.BRANCH_NAME} 分支被触发
   - 其他分支应该被过滤掉了

推荐使用：
   regexpFilterExpression: '^refs/heads/\${BRANCH_NAME}\$'
"""
                    } else {
                        report += """
❌ 方案 1 失败：\${BRANCH_NAME} 在 triggers 中不可用
   - 所有分支都被触发了
   - 需要在 stage 中进行二次校验

推荐使用方案 2 或 3：
   - 方案 2: 在 stage 中校验分支
   - 方案 3: 使用 GitLab Webhook URL 参数
"""
                    }
                    
                    report += """
========================================
🔧 下一步操作：
========================================
"""
                    
                    if (env.ref_name == env.BRANCH_NAME) {
                        report += """
✅ 您可以直接使用当前配置
✅ 无需修改 Jenkinsfile
"""
                    } else {
                        report += """
⚠️  需要修改为以下方案之一：

方案 2 (在 stage 中校验):
   stage('分支校验') {
       steps {
           script {
               if (env.ref_name != env.BRANCH_NAME) {
                   error("分支不匹配")
               }
           }
       }
   }

方案 3 (使用 URL 参数):
   GitLab Webhook URL:
   http://jenkins/invoke?token=xxx&branch=%{branch_name}
   
   regexpFilterText: '\$url_branch'
   regexpFilterExpression: '^\${BRANCH_NAME}\$'
"""
                    }
                    
                    report += "========================================"
                    
                    echo report
                    
                    // 将报告写入构建描述
                    currentBuild.description = env.ref_name == env.BRANCH_NAME ? 
                        "✅ 方案1可用" : "❌ 方案1不可用，需要方案2或3"
                }
            }
        }
    }
}
