# Project
#include <iostream>
#include <string>
#include <iomanip>
#include <fstream>
#include <stdio.h>
#include <stdlib.h>
#include <math.h>

using namespace std;

#define BUF_SIZE 1024



double eval(char *str);
double number(char *, unsigned *);
double expr(char *, unsigned *);
double term(char *, unsigned *);
double factor(char *, unsigned *);

int main()
{
	setlocale(LC_ALL, "Russian");
	char str[BUF_SIZE] = {'-','1','+','5','-','3'};
	char s[BUF_SIZE] = { '-','1','0','+','(','8','*','2','.','5',')','-','(','3','/','1',',','5',')' };
	char r[BUF_SIZE] = { '1','+','(','2','*','(','2','.','5','+','2','.','5','+','(','3','-' ,'2',')',')',')','-','(','3','/','1','.','5',')' };
	char tr[BUF_SIZE] = { '1','.','1','+','2','.','1','+','a','b','c' };

    cout << setprecision(3) << eval(str) << endl;
	cout << setprecision(3) << eval(s) << endl;
	cout << setprecision(3) << eval(r) << endl;
	cout << setprecision(3) << eval(tr) << endl;
	system("pause");
	return 0;
}

double eval(char *str)
{
	unsigned i = 0;

	return expr(str, &i);
}

double number(char *str, unsigned *idx)
{
	double result = 0.0;
	double div = 10.0;
	int sign = 1;

	while(str[*idx] == ' ')
		++*idx;

	if (str[*idx] == '-')
	{
		while (str[*idx] == ' ')
			++*idx;

		sign = -1;
		++*idx;
	}
	
		while (str[*idx] == ' ')
			++*idx;
		
		if (isalpha(str[*idx]))
		{

			cout << "некорректный ввод, строка содержит недопустимое выражение " ;
			do {
				
				cout << str[*idx];
				++*idx;
			} while (isalpha(str[*idx]));

			getchar(); getchar(); exit(-1);
		}

			while (str[*idx] >= '0' && str[*idx] <= '9')
			{
				while (str[*idx] == ' ')
					++*idx;

				result = result * 10.0 + (str[*idx] - '0');
				++*idx;
				while (str[*idx] == ' ')
					++*idx;
			}
	
	if (str[*idx] == '.'|| str[*idx] == ',')
	{
		while (str[*idx] == ' ')
			++*idx;

		++*idx;

		while (str[*idx] >= '0' && str[*idx] <= '9')
		{
			while (str[*idx] == ' ')
				++*idx;
			result = result + (str[*idx] - '0') / div;
			div *= 10.0;

			++*idx;
		}
	}

	return sign * result;
}

double expr(char *str, unsigned *idx)
{
	double result = term(str, idx);

	while (str[*idx] == '+' || str[*idx] == '-')
	{
		while (str[*idx] == ' ')
			++*idx;

		switch (str[*idx])
		{
		case '+':
			++*idx;

			result += term(str, idx);

			break;
		case '-':
			++*idx;

			result -= term(str, idx);

			break;
		}
	}

	return result;
}

double term(char *str, unsigned *idx)
{
	double result = factor(str, idx);
	double div;

	while (str[*idx] == '*' || str[*idx] == '/')
	{
		while (str[*idx] == ' ')
			++*idx;
		switch (str[*idx])
		{
		case '*':
			++*idx;

			result *= factor(str, idx);

			break;
		case '/':
			++*idx;

			div = factor(str, idx);

			if (div != 0.0)
			{
				result /= div;
			}
			else
			{
				cout << "error";
				exit(-1);
			}

			break;
		}
	}

	return result;
}

double factor(char *str, unsigned *idx)
{
	double result;
	int sign = 1;
	while (str[*idx] == ' ')
		++*idx;
	if (str[*idx] == '-')
	{
		while (str[*idx] == ' ')
			++*idx;
		sign = -1;

		++*idx;
	}
	while (str[*idx] == ' ')
		++*idx;

	if (str[*idx] == '(')
	{
		while (str[*idx] == ' ')
			++*idx;
		++*idx;

		result = expr(str, idx);

		if (str[*idx] != ')')
		{
			while (str[*idx] == ' ')
				++*idx;
			cout<< "Brackets unbalanced!";
			exit(-2);
		}

		++*idx;
	}
	else
		result = number(str, idx);

	

	return sign * result;
}
