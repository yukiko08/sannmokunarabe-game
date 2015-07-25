# sannmokunarabe-game
My first repository on GitHub.

#include <stdio.h>
#include <stdlib.h> //rand関数用
#include <time.h>//stand関数用

bool aiteru(char c){   //boolはtrue false　のみを返す

	//return '1' <= c&&c <= '9';//文字にも大きさがある
	if ('1' <= c&&c <= '9') {
		return true;
	}
	else {
		return false;
	}
}
void PrintBoard(char b[3][3]) {//ボードを表示
	printf("%c\t%c\t%c\n", b[0][0], b[0][1], b[0][2]);
	printf("%c\t%c\t%c\n", b[1][0], b[1][1], b[1][2]);
	printf("%c\t%c\t%c\n", b[2][0], b[2][1], b[2][2]);
}

char masu(int basho, char b[3][3]) {//計算により座標を表示
	int k = basho % 3;
	int l = basho / 3;
	return b[l][k];
}


enum SOROTTASTATUS {
	PLAYER,//=0
	COMPUTER//=1
};



bool Sorotta(char b[3][3]) {
	static int narabi[8][3] = {
		{ 0, 1, 2 },
		{ 3, 4, 5 },
		{ 6, 7, 8 },
		{ 0, 3, 6 },
		{ 1, 4, 7 },
		{ 2, 5, 8 },
		{ 0, 4, 8 },
		{ 2, 4, 6 }
	};
	for (int count = 0; count < 8; count = count + 1) {
		char x, y, z;
		y = masu(narabi[count][0], b);
		z = masu(narabi[count][1], b);
		x = masu(narabi[count][2], b);
		if (x == y&&x == z) {

			return true;//for文をぬける bool に値を返しているから
		}
	}
	return false;
}


SOROTTASTATUS a;//関数の外で定義すると
void game() {
	int n;
	int v = 0;
	int x = 0;
	int y = 0;

	printf("七並べを始めます\n");
	printf("1\t2\t3\n");
	printf("4\t5\t6\n");
	printf("7\t8\t9\n");

	char board[3][3] = { { '1', '2', '3' }, { '4', '5', '6' }, { '7', '8', '9' } };

	do{  //do-while文　条件によって繰り返し


		do{
			printf("○を置きたい場所の数字を入力してください\n");
		
			scanf_s("%d", &n);

			// 開いていたら　おく
			n = n - 1;
			int k, l;
			k = n % 3;
			l = n / 3;

			if (aiteru(board[l][k]) == true) {
				board[l][k] = 'o';
				x = 1;
			}
			else{
				printf("数値を入れなおしてください\n");
				x = 0;// 二回目以降はxに１が入ったままになるから
			}

		} while (x == 0);

		PrintBoard(board);

		if (Sorotta(board) == true) {
			a = PLAYER;
			goto OWARI;
		}
		printf("次はコンピューターの手です。\n");
		int m = 0;
		do{
			srand((unsigned)time(NULL));
			m = rand() % 9;
			int k, l;
			k = m % 3;
			l = m / 3;

			if (aiteru(board[l][k]) == false){//開いていなかったらくりかえし
				v = 0;
			}
			else
			{
				printf("コンピューターの手は%dです\n", m + 1);
				board[l][k] = '@';
				v = 1;
			}
		} while (v == 0);//vが０の間実行

		if (Sorotta(board) == true) {
			a = COMPUTER;
			goto OWARI;
		}


		PrintBoard(board);

	} while (Sorotta(board) == false);
OWARI:
	;


}


int main() {
	game();
	switch (a){
	case PLAYER:
		printf("あなたの勝ち\n");
		break;
	case COMPUTER:
		printf("you	の負けだよ☆\n");
		break;
	}
	return 0;
}
