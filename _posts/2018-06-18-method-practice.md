---
layout: post
title:  "method-practice"
date:   2018-06-18 12:45:00
excerpt: "main method, method chain"
project: false
tag:
- baseball
---

# main method 하나에 다 때려박기

```java
import java.util.Scanner;
class BaseBallGame{

public static void main(String[] args){
	int[] ran = new int[3];
    int[] sel = new int[3];
    Scanner sc = new Scanner(System.in);
      // 번호 생성 (0~9사이의 중복되지 않는 랜덤한 정수를 int[]형으로 반환)
      for (int i = 0; i < 3 ; i++){
      ran[i] = (int)((Math.random() * 10));
      for (int j = 0; j < 3 ; j++){
      if (ran[i] == ran[j] && i != j){
      ran[i] = (int)((Math.random() * 10));
      i--;
      break;
      }// if
    }// ifor
    }// ofor
    // 번호 맞추기 입력

    int strike = 0;
    int ball = 0;
    int count = 0;
    while (true){
    strike = 0;
    ball = 0; // 볼카운트 초기화
    for (int i = 0; i < 3 ; i++){
        System.out.print((i+1) + "번째숫자를 입력하시오: ");
        sel[i] = sc.nextInt();
        for (int j = 0; j <= i ; j++){
            if (sel[i] > 9 || sel[i] < 0){
                System.out.printf("범위를 벗어났습니다. 다시 입력해 주세요%n");
                i--;
                break;
            }else if (sel[i] == sel[j] && i !=j){
                System.out.printf("중복값을 입력하셨습니다. 다시 입력해 주세요!%n");
                i--;
                break;
            }// if
        }// ifor
    }// ofor

    for (int i = 0; i < 3 ; i++){
        System.out.printf(sel[i] + " ");
    }// 입력한 숫자를 뿌려주는 for
        System.out.println();

    for (int i = 0; i < 3; i++){
        for (int j = 0; j < 3; j++){
            if ( i == j && sel[i] == ran[j]){
                strike++;
            }
            if (i!=j && sel[i]==ran[j]){
                ball++;
            }// S와 B를 비교하는if
        }// ifor
    }// ofor
    count++; // 시도횟수 카운트 
    System.out.printf("이번 결과는 %d스트라이크 %d볼입니다. 현재%dout%n", strike, ball, count);

    if (strike == 3){
        System.out.printf("축하합니다. 당신이 승리하셨습니다. 시도횟수 %dout%n", count);
    break;
    } else if (count == 10){
        System.out.printf("안타깝지만. 당신은 패배자!.%n");
        break;
    } // 결과를 뿌려주는 if
    }// while
    }// main
    }
```

# method 분류시키기

```java

    import java.util.*;
    class BaseBallGameMethod{
    public static void main(String[] args){
        baseBall();
    } // main

    public static void baseBall(){
        int[] ran = new int[3];
        int[] sel = new int[3];
        int count = 0;

        randomNum(ran);
        while (true){
        count++;
        selNum(sel);
        int s = testNum(count, ran, sel);
        if (s == 3){
            System.out.printf("축하합니다. 당신이 승리하셨습니다.%n");
            break;
        }
        if (count == 10){
            System.out.printf("안타깝지만. 당신은 패배자!.%n");
            break;
        }// if
        }// while
    }// baseBall

    public static void randomNum(int[] ran){
        for (int i = 0; i < 3 ; i++){
            ran[i] = (int)(Math.random() * 10);
            for (int j = 0; j <= i ; j++){
                if (ran[i] == ran[j] && i !=j){
                    i--;
                    break;
                }// if
            }// ifor
        }// ofor
    }// randomNum

    public static void selNum(int[] sel){
        Scanner sc = new Scanner(System.in);
        for (int i = 0; i < 3 ; i++){
            System.out.printf("%d번째 번호를 입력해 주세요: ", i+1);
            sel[i] = sc.nextInt();
            for (int j = 0; j <= i; j++){
                if (sel[i] < 0 || sel[i] > 9){
                    System.out.printf("범위를 벗어났습니다. 다시 입력해 주세요%n");
                    i--;
                    break;
                } else if (sel[i] == sel[j] && i !=j){
                    System.out.printf("중복값을 입력하셨습니다. 다시 입력해 주세요!%n");
                    i--;
                    break;
                }// if
            }// ifor
        }// ofor
        for (int i = 0; i < 3; i++){
            System.out.printf("%d ", sel[i]);
        }
        System.out.println();
    }// selNum
    public static int testNum(int count, int[] ran, int[] sel){
        int strike = 0;
        int ball = 0;
        for (int i = 0; i < 3; i++){
            for (int j = 0; j < 3; j++){
                if (ran[i] == sel[j] && i == j){
                    strike +=1;
                } else if (ran[i] == sel[j] && i !=j){
                    ball +=1;
                }// if
            }// ifor
        }// ofor
        System.out.printf("이번 결과는 %d스트라이크! %d볼! (시도횟수: %d)%n", strike, ball, count);
        return strike;
    }// testNum
}// class


```



# class 분류시키기

## GenerateRandom  <- randomNum

```java

public class GenerateRandom{
    public static final ArrayList<Integer> numbers = new ArrayList<>(Arrays.asList(1,2,3,4,5,6,7,8,9));
    static ArrayList<Integer> resultNumbers = new ArrayList<>();
    
    public static ArrayList<Integer> shuffle(ArrayList<Integer> numbers){
        Collections.shuffle(numbers2);
        return numbers;
    }
    
    public static ArrayList<Integer> resultNumber(ArrayList<Integer> shuffle){
        for(int i=0; i<3; i++){
            resultNumbers.add(shuffle.get(i));
        }
        return resultNumbers;
    }
    
    public static int result(ArrayList<Integer> resultNumbers){
        String result = "";
        for(int i=0; i<resultNumbers.size(); i++){
            result += resultNumbers.get(i);
        }
        return Integer.parseInt(result);
    }
}

```

## input  <- selNum

```java

public class Input{
    public int input(){
        Scanner scan = new Scanner(System.in);
        return scan.nextInt();
    }
}

```

## Operator  <- testNum

```java

public class Operator{
    private final ArrayList<Integer> splitInt = new ArrayList<>();
    
    public ArrayList<Integer> splitInt(int inputNumber){
        while(inputNumber != 0){
            splitInt.add(inputNumber % 10);
            inputNumber /= 10;
        }
        Collections.reverse(splitInt);
        return splitInt;
    }
    
    public int findStrike(ArrayList<Integer> inputSplit, ArrayList<Integer> shuffle){
        int countStrike = 0;
        for(int i=0; i<inputSplit.size(); i++){
            if(inputSplit.get(i) == shuffle.get(i)){
                countStrike++;
            }
        }
        return countStrike;
    }
    
    public void resultStrike(int countNumber){
        if(countNumber == 3){
            System.out.println("3 strike!!");
        }else if(countNumber == 2){
            System.out.println("2 strike!");
        }else if(countNumber == 1){
            System.out.println("1 strike");
        }
    }
    
    public int findBall(ArrayList<Integer> inputSplit, ArrayList<Integer> shuffle){
        int countBall = 0;
        for(int i=0; i<inputSplit.size(); i++){
            if(inputSplit.contains(shuffle.get(i))){
                if(inputSplit.get(i) != shuffle.get(i)){
                    countBall++;
                }
            }
        }
        return countBall;
    }
    
    public void resultBall(int countBall){
        if(countBall == 3){
            System.out.println("3 ball");
        }else if(countBall == 2){
            System.out.println("2 ball");
        }else if(countBall == 1){
            System.out.println("1 ball");
        }
    }
    
    public void printNothing(int countBall, int countNumber){
        if(countBall == 0 && countNumber == 0){
            System.out.println("Nothing");
        }
    }
    
}

```

## Operator2  <- baseball

```java

```

## main 

```java

public class PlayGame{
    public static void main(String[] args){
        ArrayList<Integer> shuffle = GenerateRandom.shuffle(GenerateRandom.numbers);
        ArrayList<Integer> resultNumbers = GenerateRandom.resultNumber(shuffle);
        int result = GenerateRandom.result(resultNumbers);
        Input inputClass = new Input();
        
        //game play count
        int count = 0;
        try{
            while(true){
                System.out.print("Input Numbers : ");
                int inputNumbers = inputClass.input();
                Operator operator = new Operator();
                ArrayList<Integer> inputSplit = operator.splitInt(inputNumbers);
            
                //result strike, ball, out
                int countStrike = operator.findStrike(inputSplit, shuffle);
                operator.resultStrike(countStrike);
            
                int countBall = operator.findBall(intputSplit, shuffle);
                operator.resultBall(countBall);
            
                operator.printNothing(countBall, countStrike);
            
                count++;
            
                //finish
                if(inputNumber == result){
                    System.out.println("Game Finished");
                    System.out.println(count + "回で終わりです。");
                    break;
                }
            }// while
        }catch(Exception ex){
            System.err.println("Bad input "+ex.getMessage());
        }
    }
}

```

# 실무수준 코딩규약 따라서 base-ball game 업그레이드

* TUI를 게임이 끝나도/에러가 나도 밖으로 나가지 않고 여러번 반복할 수 있게 switch문 도입
* 예외처리!! 1. 숫자 외 2. 반복되는 숫자 예외  3. 1~9까지는 처리済(0은 필요없음)
* 주석 달기 - 클래스 설명, 파라미터 설명
* 메소드 처리를 함수형으로 하든지 더 간단하게
* 로그 찍기 !!!