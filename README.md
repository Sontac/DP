# 연속행렬곱셈 알고리즘

### 1. 연속행렬곱셈이란?

연속행렬곱셈은 연속된 행렬들의 곱셈에 필요한 최소 곱셈 값을 구하는 것이다.

예를 들어 4개의 행렬 A, B, C, D가 있으며

각각의 행렬의 차수를 1&#42;5, 5&#42;10, 10&#42;15, 15&#42;15라고 가정한다.

4개의 행렬의 곱셈을 여러가지 방법으로 해본다.

```
((A*B)*C)*D) = (1*5*10) + (1*10*15) + (1*15*15) = 425
A*(B*(C*D)) = (10*15*15) + (5*10*15) + (1*5*15) = 3075
(A*B)*(C*D) = (1*5*10) + (10*15*15) + (1*10*15) = 2450
(A*((B*C)*D) = (5*10*15) + (5*15*15) + (1*5*15) = 1950
```

위의 식은 곱하는 순서에 따라 425~3075번의 곱셈횟수가 나오게 되며, 그 중 최소 곱셈 횟수는 425회이다.

따라서 앞서말한 연속행렬곱셈을 알고리즘을 통해 최소한의 곱셈 횟수를 구하려 한다.

____

### 2. 연속행렬곱셈 알고리즘

##### 재귀관계식

M&#91;i&#93;&#91;j&#93; = minimum(M&#91;i&#93;&#91;k&#93; + M&#91;k+1&#93;&#91;j&#93; + d<sub>i-1</sub>d<sub>k</sub>d<sub>j</sub> (i≤k≤j-1)

M&#91;i&#93;&#91;j&#93; = 0																(if i==j)

위에 관계식을 하나씩 예를 들었을 경우

A(1&#42;5), B(5&#42;10), C(10&#42;15), D(15&#42;15) 일 때,

d<sub>0</sub>=1, d<sub>1</sub>=5, d<sub>2</sub>=10, d<sub>3</sub>=15, d<sub>4</sub>=15



1. M&#91;1&#93;&#91;2&#93; (행렬 A~B까지의 곱의 횟수) (1≤K≤1) 

    =minimum(M&#91;1&#93;&#91;k&#93; + M&#91;k+1&#93;&#91;2&#93; + d<sub>0</sub>d<sub>k</sub>d<sub>2</sub>)

    =M&#91;1&#93;&#91;1&#93; + M&#91;2&#93;&#91;2&#93; + d<sub>0</sub>d<sub>1</sub>d<sub>2</sub>

    =0 + 0 + 1&#42;5&#42;10

    =50

2. M&#91;2&#93;&#91;3&#93; (행렬 B~C까지의 곱의 횟수) (2≤K≤2) 

    =minimum(M&#91;2&#93;&#91;k&#93; + M&#91;k+1&#93;&#91;3&#93; + d<sub>1</sub>d<sub>k</sub>d<sub>3</sub>)

    =M&#91;2&#93;&#91;2&#93; + M&#91;3&#93;&#91;3&#93; + d<sub>1</sub>d<sub>2</sub>d<sub>3</sub>

    =0 + 0 + 5&#42;10&#42;15

    =750

3. M&#91;1&#93;&#91;3&#93; (행렬 A~C까지의 곱의 횟수) (1≤K≤2) 

    =minimum(M&#91;1&#93;&#91;k&#93; + M&#91;k+1&#93;&#91;3&#93; + d<sub>0</sub>d<sub>k</sub>d<sub>3</sub>)

    =minimum(M&#91;1&#93;&#91;1&#93; + M&#91;2&#93;&#91;3&#93; + d<sub>0</sub>d<sub>1</sub>d<sub>3</sub>, M&#91;1&#93;&#91;2&#93; + M&#91;3&#93;&#91;3&#93; + d<sub>0</sub>d<sub>2</sub>d<sub>3</sub>)

    =minimum(0 + 750 + 1&#42;5&#42;15, 50 + 0 + 1&#42;10&#42;15)

    =minimum(825, 200)

    =200

행렬 A~D까지의 곱의 횟수 M&#91;1&#93;&#91;4&#93;는 

M&#91;1&#93;&#91;4&#93; = minimum(M&#91;1&#93;&#91;1&#93; + M&#91;2&#93;&#91;4&#93; + d<sub>0</sub>d<sub>1</sub>d<sub>4</sub>, M&#91;1&#93;&#91;2&#93; + M&#91;3&#93;&#91;4&#93; + d<sub>0</sub>d<sub>2</sub>d<sub>4</sub>, M&#91;1&#93;&#91;3&#93; + M&#91;4&#93;&#91;4&#93; + d<sub>0</sub>d<sub>3</sub>d<sub>4</sub>)

M&#91;1&#93;&#91;4&#93;를 구하기 위해선

M&#91;1&#93;&#91;1&#93; ~ M&#91;1&#93;&#91;4&#93; 의 값 (구하려는 값의 테이블 좌측값), M&#91;2&#93;&#91;4&#93; ~ M&#91;4&#93;&#91;4&#93; (구하려는 값의 테이블 아랫값)

M&#91;i&#93;&#91;j&#93;의 값은 다음과 같이 대각선을 하나씩 증가시키며 구할 수 있다.

1) (1,1)~(4,4) , (i==j, M&#91;i&#93;&#91;j&#93; = 0)

|       |  1   |  2   |  3   |  4   |
| :---- | :--: | :--: | :--: | :--: |
| **1** |  0   |      |      |      |
| **2** |      |  0   |      |      |
| **3** |      |      |  0   |      |
| **4** |      |      |      |  0   |

2) (1,2)~(3,4)

|       |  1   |  2   |  3   |  4   |
| ----- | :--: | :--: | :--: | :--: |
| **1** |  0   |  50  |      |      |
| **2** |      |  0   | 750  |      |
| **3** |      |      |  0   | 2250 |
| **4** |      |      |      |  0   |

3) (1,3)~(2,4)

|       |  1   |  2   |  3   |  4   |
| ----- | :--: | :--: | :--: | :--: |
| **1** |  0   |  50  | 200  |      |
| **2** |      |  0   | 750  | 1875 |
| **3** |      |      |  0   | 2250 |
| **4** |      |      |      |  0   |

4) (1,4)

|       |  1   |  2   |  3   |  4   |
| ----- | :--: | :--: | :--: | :--: |
| **1** |  0   |  50  | 200  | 425  |
| **2** |      |  0   | 750  | 1875 |
| **3** |      |      |  0   | 2250 |
| **4** |      |      |      |  0   |

# Code


## 1. 소스코드



### Bottom-up 구상
> 처음 구간 간격  i 를 설정한 후 그 구간 길이에 따라 구할 수 있는 행렬의 곱셈을 구한 뒤 값을 저장하는 방식

```JAVA
public class ChainedMatrixMultiplications {

    static int n, INF = Integer.MAX_VALUE;
    static int[] data;
    static int [][] dp = new int[n][n];

    public static void main(String [] args) throws IOException{
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        n = Integer.parseInt(br.readLine());

        data = new int[n+1];
        StringTokenizer st = null;
        for(int i=0; i<n; i++) {
            st = new StringTokenizer(br.readLine());
            int a = Integer.parseInt(st.nextToken());
            int b = Integer.parseInt(st.nextToken());
            data[i] = a; data[i+1] = b;
        }

        dp = new int[n][n];

        for(int i=1; i<n; i++) {
            for(int j=0; j<n-i; j++) {
                dp[j][j+i] = INF;
                for(int k=0; k<i; k++) {
                    int cost = dp[j][k]  + dp[k+1][j+i-1] + (data[j]*data[k+1]*data[j+i]);
                    dp[j][j+i] = Math.min(dp[j][j+i], cost);
                }
            }
        }
        System.out.println(dp[0][n-1]);
    }

}
```
### Top-down 구상

```JAVA
public class ChainedMatrixMultiplications {
    
    static int n, INF = Integer.MAX_VALUE;
	static int[] data;
	static int[][] dp;
	public static void main(String[] args) throws IOException{
		BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
		n = Integer.parseInt(br.readLine());
		
		data = new int[n+1];
		StringTokenizer st = null;
		for(int i=0; i<n; i++) {
			st = new StringTokenizer(br.readLine());
			int a = Integer.parseInt(st.nextToken());
			int b = Integer.parseInt(st.nextToken());
			data[i] = a; data[i+1] = b;
		}
		
		dp = new int[n][n];
		for(int i=0; i<n; i++) {
			Arrays.fill(dp[i], INF);
		}
		System.out.println(solve(0,n-1));
	}
	static int solve(int pos, int cur) {
		if(pos == cur) return 0;
		if(dp[pos][cur] != INF) return dp[pos][cur];
		
		for(int i=pos; i<cur; i++) {
			int value = solve(pos,i) + solve(i+1, cur) + (data[pos] *data[i+1]*data[cur+1]);
			dp[pos][cur] = Math.min(dp[pos][cur], value);
		}
		
		return dp[pos][cur];
	}

}
```


## 2. 결과창


```
4
1 5
5 10
10 15
15 15
425
```