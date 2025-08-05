pipeline {
    agent any

    stages {
        stage('Compare Branches') {
            steps {
                script {
                    // Compare dev and uat branches and fetch merged PRs
                    def mergedPRs = sh(script: '''
                        git fetch origin
                        git checkout uat
                        git log --merges --oneline dev..uat
                    ''', returnStdout: true).trim()
                    
                    // Process merged PRs (this part will need actual parsing logic)
                    // For now, let's assume mergedPRs is a parsed list
                    def releaseNotes = generateReleaseNotes(mergedPRs)
                    
                    // Write release notes to RELEASE_NOTES.md
                    writeFile file: 'RELEASE_NOTES.md', text: releaseNotes
                }
            }
        }
        stage('Commit Release Notes') {
            steps {
                script {
                    // Commit the release notes
                    sh 'git add RELEASE_NOTES.md'
                    sh 'git commit -m "Add release notes for the latest PRs"'
                    
                    // Push changes or create a PR
                    sh 'git push origin uat || echo "Failed to push, creating PR instead."'
                }
            }
        }
    }
}

def generateReleaseNotes(mergedPRs) {
    // Logic to generate formatted release notes
    // This will return a markdown formatted string
    return "# Release Notes\n\n" + mergedPRs.collect { pr ->
        "- PR #"+{pr.number}+": "+{pr.title}+" by "+{pr.author}+"\n  Description: "+{pr.description}"
    }.join("\n")
}