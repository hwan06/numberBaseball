# 숫자 야구 게임
## 1. [문제]
**무작위 난수 3개를 컴퓨터로부터 받아 사용자는 무작위 난수 3개의 순서와 수까지 맞추면 되는 간단한 게임.**   
**(총 11번 진행하며, 난수는 1부터 9사이에 있는 수여야하고, 서로 달라야함.)**
## 2. [구현을 위한 메소드들]
**1. 인수가 없는 무작위 난수 발생 메소드**     
**2. 인수가 있는 사용자 입력 메소드**    
**3. 메인 메소드**    
**※이때 인수가 있는 메소드와 없는 메소드의 이름이 같은데, 이는 객체지향에서 가능한 오버로딩의 특징이다**
## 3. [구현]
**1. 먼저 컴퓨터로부터 난수 3개를 받기 위해 인수 없는 메소드 playGame()를 생성한다.**    
**1부터 9사이여 하기 때문에 9로 나눈 나머지에 1을 더해주고,**   
**do-while문을 통해 x,y,z 모두 다른 세개의 난수를 생성해준다.**
``` java
public static int playGame() throws IOException {

		Random r = new Random(); // 난수 발생을 위한 Random 선언
		int x, y, z;

		x = Math.abs(r.nextInt() % 9) + 1; // 1부터 9사이의 수를 구하기 위함.

		do {
		  y = Math.abs(r.nextInt() % 9) + 1; 
		} while (x == y); // x와 같은지 비교

		do {
		  z = Math.abs(r.nextInt() % 9) + 1;
		} while (z == x || z == y); // x,y와 같은지 비교


		return playGame(x, y, z); // 리턴값
	}
```
**2. 메소드와 변수들 생성**
``` java
public static int playGame(int x, int y, int z) throws IOException {

		BufferedReader in = new BufferedReader(new InputStreamReader(System.in));

		int count; // 게임 횟수
		int strike, ball; // 스트라이크와 볼 카운팅을 위해 변수 생성
		String user; // 사용자로부터 입력 받을 수. 밑에서 int형으로 바꾸어 저장
		int[] com = { x, y, z }; // 위에 있는 playGame()로부터 받은 x,y,z값 저장
		int[] usr = new int[3]; // 사용자로부터 입력받은 수 저장
		count = 0; 
```
**3. 반복문을 통해 사용자에게 입력 받고 정수형으로 변환**
``` java
do { // 게임 실행 코드
				
	for(int i = 1; i < 4; i++) { 
		System.out.println(i + "번째 숫자를 입력하세요"); 
		user = in.readLine(); // 사용자에게 String형으로 입력을 받고,
		usr[i-1] = new Integer(user).intValue();  // usr 배열방에 저장
			}
```
**4. do-while문을 통해 수가 모두 정해 질때까지 반복하며 입력받은 수가 조건에 맞는지 확인하고 아니면 알려주기**
``` java
do {
	if((usr[0] == 0) || (usr[1] == 0) || (usr[2] == 0)) { // 0인지 확인
		System.out.println("0은 입력이 안됩니다. 다시 입력해주세요.");
	} 
				
	else if((usr[0] > 9) || (usr[1] > 9) || (usr[2] > 9)) { // 9보다 큰지 확인
		System.out.println("1부터 9 사이의 숫자만 가능합니다. 다시 입력해주세요.");
	} 
				
	else if((usr[0] == usr[1]) || (usr[1] == usr[2]) || (usr[0] == usr[2])) { // 모두 다른 수 인지 확인
		System.out.println("세가지 모두 다른 숫자만 가능합니다. 다시 입력해주세요.");
	}
	} while ((usr[0] == 0) || (usr[1] == 0) || (usr[2] == 0) || 
					(usr[0] > 9) || (usr[1] > 9) || (usr[2] > 9)|| 
					(usr[0] == usr[1]) || (usr[1] == usr[2]) || (usr[0] == usr[2]));
				
```
**5. 세가지 수가 모두 정해져 do-while문을 빠져나왔다면, if문을 통해 strike, ball 카운팅하여 출력해주기**     
**만약 strike가 3이거나 count가 11이라면 게임 종료**
``` java
strike = ball = 0;
			
	// 컴퓨터에게 받은 인수와 입력받은 인수가 숫자와 자리 모두 같다면 strike 1증가 //
			if (usr[0] == com[0]) strike++; 
			if (usr[1] == com[1]) strike++;
			if (usr[2] == com[2]) strike++;
	
	// 컴퓨터에게 받은 인수와 입력받은 인수가 자리만 같다면 ball 1증가//
			if (usr[0] == com[1]) ball++;
			if (usr[0] == com[2]) ball++;
			if (usr[1] == com[0]) ball++;
			if (usr[1] == com[2]) ball++;
			if (usr[2] == com[0]) ball++;
			if (usr[2] == com[1]) ball++;

			// ball과 strike 출력
			System.out.println("Ball: " + ball + " " + "Strike: " + strike);

		} while (strike < 3 && count < 11); // strike가 3이거나, count가 11이라면 종료

		return count;
	}
```
**6. result를 생성하여 커맨드 라인에서 받은 아규먼트의 길이가 3이라면 바로 플레이어에게 입력을 받고 그 외에는 무작위 난수 발생 메소드를 호출하여 게임을 진행.**    

``` java
public static void main(String[] args) throws IOException {

		int result; // 게임 진행 횟수 저장

		if (args.length == 3) { // 아규먼트의 길이가 3이라면

			int x = Integer.valueOf(args[0]).intValue(); 
			int y = Integer.valueOf(args[1]).intValue();
			int z = Integer.valueOf(args[2]).intValue();

			result = playGame(x, y, z); // 입력받은 3개를 인수가 있는 playGame()로 바로 보내 게임 플레이
		}
		// 그 외에는
		else { 
			result = playGame(); // 무작위로 난수를 발생하여 게임 플레이
		}
```
**아규먼트란 커맨드 라인에서 인자(argument)를 받아와 게임을 실행하는 것으로, 게임 진행자가 직접 입력한다.**     
**이클립스 기준 경로: 왼쪽 상단에 위치한 Run-> Run configuration-> argument**      
![image](https://github.com/hwan06/numberBaseball/assets/114748934/26817854-51f7-4800-980b-e7e84bb5a7cc)

**7. 마지막으로 게임 진행 횟수에 따른 결과 출력**
``` java
		// 진행 횟수가 2 이하
		if (result <= 2) { 
			System.out.println("참 잘했어요");
		} 
		// 진행 횟수가 5 이하
		else if (result <= 5) {
			System.out.println("잘했어요!");
		} 
		// 진행 횟수가 9 이하
		else if (result <= 9) {
			System.out.println("보통이네요!");
		} 
		// 나머지(실패 포함)
		else {
			System.out.println("분발하세요!");
		}
	}
}
```
