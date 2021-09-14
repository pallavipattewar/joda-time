def name;
pipeline {
    agent any
    	stages {
       		 stage('Build Stage') {
          		 		steps {
               					 bat "mvn -B -DskipTests clean package"
                 			      }
        			      }
        	stage('Testing Stage') {
					steps {
						script {
							def count = matchKeywordCount()
							println "count above"+count
				
							if(count > 0) 
							{
								bat "mvn -Dsuite=PerformanceTests test"
					
                   		 			}
							else
							{
							bat "mvn -Dsuite=FunctionalTests test"
							}
						      }
						}
						post{
                         				always{
                              					junit "**/target/surefire-reports/TEST-org.joda.time.TestAllPackages.xml"
                       					      }
                     				    }
						}
   					 }
}


def selectCommits(){

    // Retrieve all the commits and select 2 consecutive commit codes from it

    def commitCode = bat (script: 'git log --format=format:"%%H"', returnStdout: true).trim()
    String[] hashCode = null;

    hashCode = commitCode.split("\n")
    println Arrays.toString(hashCode)

    Random r = new Random()
    int n1 = r.nextInt(hashCode.size())
    println n1
    int n2 = n1-1
    println n2
    
    println "First hashcode"+hashCode[n1+1] 
    println "Second hashcode"+hashCode[n2+1]

	String[] selectedCommitArray = new String[2];
	selectedCommitArray[0] = hashCode[n1+1];
	println "1stcommitinarray"+selectedCommitArray[0]
	selectedCommitArray[1] = hashCode[n2+1];
	println "2ndcommitinarray"+selectedCommitArray[1]
	return selectedCommitArray;
	
}

def commitDifference(){
 	
    // Get difference between two selected commits
	String[] commits = selectCommits()
	def firstCommit = commits[0]
	def secondCommit = commits[1]
	println firstCommit
	println secondCommit
	
	//def result = bat (script: "git diff -u $firstCommit $secondCommit | grep -E '^\\+'",returnStdout: true).trim()
	//def result = bat (script: "git diff -u $firstCommit $secondCommit | find /i /"+/"",returnStdout: true).trim()
	def result = bat (script: "git diff $firstCommit $secondCommit find /i '^\\+'",returnStdout: true).trim()
	//String repl = result.replaceAll("(\\r|\\n|\\r\\n|\\r|,)+", "\\\\n")
	
    println(result)

   // String diff = rep1.toString().toLowerCase()
   // println diff
	//return diff
}

def matchKeywordCount() {
	String difference = commitDifference()
    	String[] diffArray = null;
	String[] keywords = ["Runtime", "New", "gc", "System"];
	      
	int count =0;  
	        
	        diffArray = difference.split(" ");
	        for(int i=0 ;i< diffArray.length ;i++) {
	        	for(int j=0 ;j < keywords.length ; j++ )
	        	{
	        	 //if(((diffArray[i].contains(keywords[j])) || (diffArray[i].startsWith(keywords[j])))
			if((diffArray[i].contains(keywords[j])))
	        	{
	        		count++;
	        	}
	        	}
	        }
	println "below count"+count
	return count
}
			    
def createCSV(){
	
        //CSV code start
	
	def newFile = new File("D:\\Test.csv")
        def exists = fileExists 'D:\\Test.csv'
        println exists

        if (!exists) {
             newFile.append("HashCode, Random HashCode 1, Random HashCode 2, Diff. between two commits, Code change category, Test case type,Total no. of test cases, No. of succeeed test, No. of failed tests, \n")
    
                    }

    def currentHashcode = bat (script: '@git log -1 --pretty=%%H',returnStdout: true).trim()
	
	String[] commits = selectCommits()
	def firstCommit = commits[0]
	def secondCommit = commits[1]
	
	String difference = commitDifference()
	
	def count = matchKeywordCount()
	
	// for print code change category
	def codeChangeCategory 
	def testCaseType
	if(count >= 1)
	{
		codeChangeCategory = "Memory Management"
		testCaseType = "Performance Test"
	}
	else
	{
		codeChangeCategory = "Functional"
		testCaseType = "Functional Test"
	}
	
	newFile.append("\n")
	newFile.append("${currentHashcode}, ${firstCommit}, ${secondCommit}, ${difference}, ${codeChangeCategory}, ${testCaseType}")
  
    //csv code end

	      
}
