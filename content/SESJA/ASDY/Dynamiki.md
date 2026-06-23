![[obrazki/Pasted image 20260617125503.png]]

## ZAD 1

```python
procedure isNAchievable(S -> int[] , N -> int)
int l = S.length
dp = bool[N+1, l+1]
for(int i = 0; i <= l ; i++){
	dp[0,i] = 1;
}
for(int j = 1; j <=l; j++){
	for(int i = 0; i<=N; i++){
		if(dp[i,j-1] == 1 ||(i > S[j] && dp[i-S[j],j-1]) == 1)
			dp[i,j] = 1
	}
}
return dp[N,j] == 1
```

## ZAD 2

```python
fun wspolnyPodciag(x, y)
n = x.length
m = y.length

length = int[n+ 1, m+1]


for(int i = 1; i <=n; i++){
	for(int j = 1; j <=m; j++){
		int max = math.max(dp[i-1,j], dp[i,j-1]);
		
		if(x[i-1] == y[j-1]){
			dp[i,j] = dp[i-1,j-1] + 1;
		}
		else{
			dp[i,j] = max;
		}
	}
}
return dp[n,m];
```

## ZAD 3

```python
fun lamanietekstu(arr, s) - array to slowa, s to dlugosc linii

int n = arr.length;

int[] dp = new int[n+1]
dp = {int.max}; //pseudokod, kazdy ma taka wartosc
dp[0] = 0;
int[] from = new int[n+1];
from = {-1};

for(int i = 1; i <=n; i++){
	int j = i-1;
	int len = 0;
	while(j >= 0){
		len += arr[j].length;
		if(len > s) break;
		if(dp[j] + (s - len) * (s - len) < dp[i]){
			dp[i] = dp[j] + (s- len) * (s - len);
			from[i] = j;
		}
		j--;
	}
}

List<int> last_pos = new list();
int i = n;
while(i > 0){
	last_pos.add(from[i]);
	i = from[i];
}
return last_pos.Reverse;
```

# ZAD 4

```python
fun (x,y)
int n = x.length;
int m = y.length;

int dp = new int[n+1 , m + 1];
for(int i = 0; i <=n; i++){
	dp[i,0] = i;
}
for(int j = 0; j<=m; j++){
	dp[0,j] = j;
}

for(int i = 1; i <= n; i++){
	for(int j = 1; j<=m; j++){
		if(x[i-1] == y[j-1]){
			dp[i,j] = dp[i - 1, j -1];
		}
		else{
			dp[i,j] = math.min(dp[i-1,j] + 1, dp[i,j-1] + 1);
		}
	}
}
return dp[n,m];
```
