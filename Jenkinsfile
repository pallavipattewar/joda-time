def name;
pipeline {
    agent any
    stages {
        stage('Build Stage') {
            steps {
                script{
                bat "mvn -B -DskipTests clean package"
                  }
            }
        }
        stage('Testing Stage') {
            steps {
                script{
                    //def result = getCommitName()
                    //String s = result.toString()
                    //demo()
                    //csv()
			demo()
                          
            }
        }
    }
}
}
//trial
def csv()
{

    def newFile = new File("D:\\Test.csv")
    def exists = fileExists 'D:\\Test.csv'
    println exists

if (!exists) {
    newFile.append("HashCode, Random HashCode 1, Random HashCode 2, Diff. between two commits, Code change category, Test case type,Total no. of test cases, No. of succeeed test, No. of failed tests, \n")
    
}
def a = bat (script: '@git log -1 --pretty=%%H',returnStdout: true).trim()

//newFile.append("${a}, ${b} \n")
	newFile.append("${a}\n")
  
}

def demo(){
    def commitCode = bat (script: 'git log --format=format:"%%H"', returnStdout: true).trim()
    String[] hashCode = null;

    hashCode = commitCode.split("\n")
    println Arrays.toString(hashCode)

    Random r = new Random()
    int n1 = r.nextInt(hashCode.size())
    println n1
    //int n2 = r.nextInt(hashCode.size())
	int n2 = n1+1
    println n2
    
    println "First hashcode"+hashCode[n1+1] 
    println "Second hashcode"+hashCode[n2+1]

    def firstCommit = hashCode[n1+1]
    def secondCommit = hashCode[n2+1]

 

    def result = bat (script: "@git diff $firstCommit $secondCommit",returnStdout: true).trim()
    println(result)

    String diff = result.toString().toLowerCase()
    println diff
    String[] diffArray = null;
	String[] keywords = ["Runtime", "New", "gc", "System"];
	      
	int count =0;
	         
	        
	        diffArray = diff.split(" ");
	        for(int i=0 ;i< diffArray.length ;i++) {
	        	for(int j=0 ;j < keywords.length ; j++ )
	        	{
	        	 if((diffArray[i].contains(keywords[j])))//|| (diffArray[i].startsWith(keywords[j])))
	        	{
	        		count++;
	        	}
	        }
	        }
	
	
	//CSV code start
	def newFile = new File("D:\\Test.csv")
    def exists = fileExists 'D:\\Test.csv'
    println exists

if (!exists) {
    newFile.append("HashCode, Random HashCode 1, Random HashCode 2, Diff. between two commits, Code change category, Test case type,Total no. of test cases, No. of succeeed test, No. of failed tests, \n")
    
}
def currentHashcode = bat (script: '@git log -1 --pretty=%%H',returnStdout: true).trim()

//newFile.append("${a}, ${b} \n")
	if(count >= 1)
	{
		def codeChangeCategory = "Memory Management"
		println codeChangeCategory
	}
	else
	{
		def codeChangeCategory = "Functional"
		println codeChangeCategory
	}
	println "outer" +codeChangeCategory
	newFile.append("${currentHashcode}, ${firstCommit}, ${secondCommit}, ${codeChangeCategory}\n")
  
	
	//csv code end
	
	        if(count > 0) {
	         bat "mvn -Dsuite=PerformanceTests test"
                       /* post{
                            always{
                                junit "**/ /*target/surefire-reports/TEST-org.joda.time.TestAllPackages.xml"
                              
                                 }
                            }	*/
	        }
	        else{
                bat "mvn -Dsuite=FunctionalTests test"
                      /*  post{
                            always{
                                junit "**/ /*target/surefire-reports/TEST-org.joda.time.TestAllPackages.xml"
                               
                                  }
                            }*/
            }
        		   
	         
	}