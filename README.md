## Refactoring

src\probono\service\TalentDonationProjectService.java

- 원본 코드
```
 /**
 * Project 이름으로 검색 - 객체된 Project 반환하기
 * 
 * @param projectName 프로젝트 이름
 * @return TalentDonationProject 검색된 프로젝트
 */
public TalentDonationProject getDonationProject(String projectName) {
  
  for (TalentDonationProject project : donationProjectList) {
    if (project != null && project.getTalentDonationProjectName().equals(projectName)) {
      return project; //메소드 자체의 종료
    }
  }

  return null;
}
```


- 리팩토링 코드
```
 /**
 * Project 이름으로 검색 - 객체된 Project 반환하기
 * 
 * @param projectName 프로젝트 이름
 * @return TalentDonationProject 검색된 프로젝트
 */
public TalentDonationProject getDonationProject(String projectName) {
  
  return donationProjectList.stream()
      .filter(project -> project != null && project.getTalentDonationProjectName().equals(projectName))
      .findFirst()
      .orElse(null);
}
```

## Refactoring Changes

### `EndView.java`

#### Original Code

```java
//진행중인 모든 프로젝트 출력
public static void projectListView(ArrayList<TalentDonationProject> allProbonoProject){
    int index = 1;
    for (TalentDonationProject project : allProbonoProject) {
        if (project != null) {
            System.out.println("[진행 중인 project : " + (index++) + "] " + project);
        }
    }
}

//Refactored Code
//진행중인 모든 프로젝트 출력
public static void projectListView(ArrayList<TalentDonationProject> allProbonoProject){
    AtomicInteger index = new AtomicInteger(1);
    allProbonoProject.stream()
        .filter(project -> project != null)
        .forEach(project -> System.out.println("[진행 중인 project : " + index.getAndIncrement() + "] " + project));
}



