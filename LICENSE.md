//программа именно транспонирует матицу. т.е. переворичивает кубиком
#include "stdafx.h"
#include <iomanip>
#include <iostream>
#include <Windows.h>
using namespace std;

//Внимание! Возможна некоректная работа, если кол-во строк НЕ равно кол-во столбцов
const int n=6; //кол-во строк
const int m=6; //кол-во столбцов
      int **mas;
	  int **reserv;

enum consolecolor
{
Black  =	0,
Red	   =	12,
Yellow =	14,
LightGray = 7
};

void SetColor(consolecolor text, consolecolor background)
{
HANDLE hStdOut = GetStdHandle(STD_OUTPUT_HANDLE);
SetConsoleTextAttribute(hStdOut, (WORD)((background << 4) | text));
}

//процедура заполнения массива
void zap (int **a, int m, int n)
{
	for(int i=0; i<=m-1; i++)
	{
		for(int k=0; k<=n-1; k++)
		{
			a[i][k]=(rand()%9)+1;
		}
	}
}

//процедура печати
void printm (int **a, int m, int n)
{
	for(int i=0; i<=m-1; i++)
	{
		for(int k=0; k<=n-1; k++)
		{
			cout<<a[i][k]<<" ";
		}
		cout<<endl;
	}
}

//процедура резервного копирования массивов для переворачивания кубиком
void copy (int **a,int **b, int m, int n)
{
	for(int i=0; i<=m-1; i++)
	{
		for(int k=0; k<=n-1; k++)
		{
			b[i][k]=a[i][k];
		}
	}
}

//процедура смены одной строки на столбец будет переворота
void zam(int **osn, int **res, int str, int stlb, int m, int n)
{
	int z=n-1,k=0;
	while(z!=-1)
	{
			res[z][stlb]=osn[str][k];	
			z--;
			k++;
	}
	

}

typedef void (*line)(int **, int, int);
line nab[]={zap, printm};

typedef void (*dual)(int **,int **, int,int);
dual nab2[]={copy};

typedef void (*global)(int **,int **, int,int,int,int);
global nab3[]={zam};

int _tmain(int argc, _TCHAR* argv[])
{
setlocale(LC_ALL, "Rus"); 
srand(time(0));
SetColor(Red, Black); cout<<"Исходная матрица"<<endl; SetColor(LightGray, Black);
	mas=new int*[n];
	for(int i=0; i<=m-1; i++)
	{
		mas[i]=new int[m];
	}

		reserv=new int*[n];
		for(int i=0; i<=m-1; i++)
		{
			reserv[i]=new int[m];
		}
	nab[0](mas, m, n);
	nab[1](mas, m, n);
	nab2[0](mas,reserv,m,n);
	cout<<endl;
	
	//НАЧАЛО ТРАНСПОНИРОВАНИЯ!!! (можно и вручную)
	for(int i=0; i<=m-1; i++)
	{
	nab3[0](mas, reserv, i, i, m ,n);
	}

	SetColor(Red, Black); cout<<"Транспонированная матрица"<<endl; SetColor(LightGray, Black);
	nab[1](reserv, m, n);



cin.get(); 
return 0;
}
