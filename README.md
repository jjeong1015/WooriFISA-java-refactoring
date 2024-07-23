## :joystick:	Refactoring Changes

2024-07-22 | [허예은](https://github.com/yyyeun), [이정민](https://github.com/jjeong1015), [최나영](https://github.com/na-rong)
### :star2: 프로젝트 개요
- 재능 기부자와 수혜자 매칭 프로그램

### :collision: 수행 과제
- Stream API와 Lambda 활용하기

### `TalentDonationProjectService.java`

#### Original Code & Refactored Code
```java
 /*
 * Project 이름으로 검색 - 객체된 Project 반환하기
 * @param projectName 프로젝트 이름
 * @return TalentDonationProject 검색된 프로젝트
 */
public TalentDonationProject getDonationProject(String projectName) {

  //Original Code
  for (TalentDonationProject project : donationProjectList) {
    if (project != null && project.getTalentDonationProjectName().equals(projectName)) {
      return project; //메소드 자체의 종료
    }
  }
  return null;

  //Refactored Code
  return donationProjectList.stream()
      .filter(project -> project != null && project.getTalentDonationProjectName().equals(projectName))
      .findFirst()
      .orElse(null);
}
```

```java
public void donationProjectUpdate(String projectName, Donator people) throws Exception {

  ////Original Code
		for (TalentDonationProject project : donationProjectList) {

			if (project != null && project.getTalentDonationProjectName().equals(projectName)) {

				if (people != null) {
					project.setProjectDonator(people);
					break;
				} else {
					throw new Exception("프로젝트 이름은 있으나 기부자 정보 누락 재확인 하세요");
				}

			} else {
				throw new Exception("프로젝트 이름과 기부자 정보 재 확인 하세요");
			}
		}
	}

public void donationProjectUpdate(String projectName, Donator people) throws Exception {
  //Refactored Code
		donationProjectList.stream()
				.filter(project -> project != null && project.getTalentDonationProjectName().equals(projectName))
				.findFirst().ifPresentOrElse(project -> {
					if (people != null) {
						project.setProjectDonator(people);
					} else {
						// Exception이 아니고 RuntimeException인 이유 : 람다 표현식 내에서 검사된 예외를 던질 수 없으므로
						// RuntimeException을 사용
						throw new RuntimeException("프로젝트 이름은 있으나 기부자 정보 누락 재확인 하세요");
					}
				}, () -> {
					throw new RuntimeException("프로젝트 이름과 기부자 정보 재 확인 하세요");
				});

	}
```

### `EndView.java`

#### Original Code & Refactored Code

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
```

### 고찰
- 허예은 : 리팩토링 과정을 통해 Stream API와 Lambda에 대해 더 잘 이해할 수 있었습니다.
- 이정민 : 리팩토링을 하며 람다식 문법에 대해 파악할 수 있는 좋은 기회였습니다.
- 최나영 : stream을 사용하여 가독성이 좋아지고 코드가 간결해져서 앞으로도 많이 활용할 수 있을 것 같습니다.


