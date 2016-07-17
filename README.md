#include<stdio.h>
#include<stdlib.h>
#include<math.h>
#include<string.h>

int main()
{
	char string[1000] = { 0 };
	char tempString[6] = { 0 };
	int position = 0;

	int numbersArr[100] = { 0 };
	int newNumberArr[100] = { 0 };
	int numberPlace = 0;

	char actArr[100] = { 0 };
	char newActArr[100] = { 0 };
	int charPlace = 0;
	
	int x = 0;
	int result = 0;
	unsigned int endAnswer = 0;

	int i, j;
	int count = 0;

	printf("y = ");
	fgets(string, 1000, stdin);
	string[strcspn(string, "\n")] = 0;

	if (xInString(string))
	{
		printf("Enter the value. x = ");
		scanf("%d", &x);
	}

	// Putting the numbers and the operations in arrs.
	for (i = 0; i < (strlen(string) + 1); i++)
	{
		if (string[i] != '+' && string[i] != '-' && string[i] != '*' && string[i] != '/' && string[i] != '^' && string[i] != NULL)
		{
			tempString[position] = string[i];										// Each number (string type).
			position++;
		}
		else
		{
			if (xInString(tempString))												// If there is 'x' in the number. For example: '10x'
			{
				for (j = 0; j < strlen(tempString); j++)
				{
					if (tempString[j] == 'x')
					{
						tempString[j] = NULL;										// Remove the 'x' from the number.
						numbersArr[numberPlace] = atoi(tempString);					// Casting the string number to integer.
						numberPlace++;
						numbersArr[numberPlace] = x;								// Adding also the value of 'x' as the next number.
						numberPlace++;

						actArr[charPlace] = '*';									// First addind '*' operation because '10x' is '10 * x'
						charPlace++;

						actArr[charPlace] = string[i];								// Finally adding the operation that is exists after the numebr.
						charPlace++;
					}
				}
			}
			else
			{
				numbersArr[numberPlace] = atoi(tempString);							// Casting the string number to integer.
				numberPlace++;

				actArr[charPlace] = string[i];
				charPlace++;
			}

			memset(tempString, 0, sizeof(tempString));								// Restting the temp string and the place.
			position = 0;
		}
	}

	// Calculate the exponentiation operation.
	for (i = 0; i < numberPlace; i++)
	{
		if (actArr[i] == '^')
		{
			result = pow(numbersArr[i], numbersArr[i + 1]);

			newNumberArr[count] = result;
			newActArr[count] = actArr[i];
		}
		else
		{
			newNumberArr[count] = numbersArr[i];
			newActArr[count] = actArr[i];
		}

		count++;
	}

	// Changing the arrs and resetting the new arrs.
	for (i = 0; i < (count - 1); i++)
	{
		actArr[i] = newActArr[i];
	}
	for (i = 0; i < numberPlace; i++)
	{
		numbersArr[i] = newNumberArr[i];
	}
	count = 0;
	memset(newNumberArr, 0, sizeof(newNumberArr));
	memset(newActArr, 0, sizeof(newActArr));

	// Calculate the multiplication and division operations.
	for (i = 0; i < numberPlace; i++)
	{
		if (actArr[i] == '*' || actArr[i] == '/')
		{
			result = numbersArr[i];
			do																		// For cases like '2 * 5 / 2'.
			{
				switch (actArr[i])
				{
				case '*':
					result *= numbersArr[i + 1];
					break;
				case '/':
					result /= numbersArr[i + 1];
					break;
				}
				i++;
			} while (actArr[i] == '*' || actArr[i] == '/');

			newNumberArr[count] = result;
			newActArr[count] = actArr[i];
		}
		else
		{
			newNumberArr[count] = numbersArr[i];
			newActArr[count] = actArr[i];
		}

		count++;
	}

	// Calculate all the others operations.
	endAnswer = newNumberArr[0];
	for (i = 0; i < count; i++)
	{
		switch (newActArr[i])
		{
		case '+':
			endAnswer += newNumberArr[i + 1];
			break;
		case '-':
			endAnswer -= newNumberArr[i + 1];
			break;
		}		
	}

	printf("y = %d \n", endAnswer);

	system("PAUSE");
	return 0;
}

int xInString(char string[])
{
	int i = 0;

	for (i = 0; i < strlen(string); i++)
	{
		if (string[i] == 'x')
		{
			return 1;
		}
	}

	return 0;
}
