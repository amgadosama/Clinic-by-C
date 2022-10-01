# Clinic-by-C
A clinic management system by C



#include <stdio.h>
#include "STD_types.h"
#include <stdlib.h>
#include <string.h>
typedef struct nodetype node;
struct nodetype
{
	u8* name;
	u8* gender;
	u8 age;
	u8 slot;
	u32 ID;
	node* tail;
};
node* head;
u32 listlength=0;
u32 addnode(u8* name,u8* gender,u8 age,u32 ID);
u32 editpatient(u32 ID);
u32 reserveslot(u32 ID);
u32 patientrecord(u32 ID);
int main (void)
{
	head=(node*)malloc(sizeof(node));
	u16 choose;
	u8* name=(u8*)malloc(sizeof(u8));
	u8* gender=(u8*)malloc(sizeof(u8));
	u8 age;
	u32 ID;	
	int flag;
	u8 scan;
	while(1)
	{	// --------------------Admin mode or user moder------------------//
		u8 Exit=0;
		printf("Choose between admin mode 1 or user mode 2: ");
		scanf("%d",&choose);
		if(1==choose)
		{ // -----------------------------Admin mode---------------//
			u16 pw;
			u8 i=0;
			printf("Enter PW: ");
			scanf("%d",&pw);
			while (i<2 && Exit != 1)
			{
				if(1234==pw)
				{
					printf("Enter 1 for adding new patient \nEnter 2 for editing patient record \nEnter 3 to Reserve a slot with doctor \nEnter 4 to cancel reservation \nEnter 5 to Exit ");
					scanf("%d",&scan);
					switch  (scan)
					{
							case 1:
							//------------------------------Adding new patient-------------------//
							printf("please enter name: ");
							fflush(stdin);
							gets(name);
							printf("please enter gender: ");
							fflush(stdin);
							gets(gender);
							printf("please Enter age: ");
							scanf("%d",&age);
							printf("Enter ID: ");
							scanf("%d",&ID);
							flag=addnode(name,gender,age,ID);
							if(flag==1)
							{
								//check if ID existed or not
								printf("----------------ID existed please try again---------------- \n");
							}
							else
							{
								printf("Patient name: %s \n", name);
								printf("Patient gender: %s \n",gender);
								printf("Patient age: %d \n",age);
								printf("Patient ID: %d \n",ID);
								printf("----------------Patient added successfully----------------\n");
							}
							flag =0;
							break;
							case 2:
							// ----------------------Editing patient info by ID -----------------//
							printf("Enter ID to edit patient info \n");
							scanf("%d",&ID);
							flag=editpatient(ID);
							if(1==flag)
							{
								printf("----------------Patient edited successfully---------------- \n");
								flag=0;
							}
							break;
							case 3:
							printf("Slots are\n1. 2 to 2:30\n2. 2:30 to 3\n3. 3 to 3:30\n4. 4 to 4:30\n5. 4:30 to 5\n");
							printf("Enter ID to Reserve slots: ");
							scanf("%d",&ID);
							flag=reserveslot(ID);
							if(1==flag)
							{
								printf("----------------Slot Reserved successfully---------------- \n");
							}
							case 5:
							Exit=1;
							break;
							
					}	
					
				}
				else 
				{
					printf("----------------Please Try again ----------------\n");
					scanf("%d",&pw);
					i++;
					if(i==2)
					{
						printf("Incorrect PW for 3 conscutive time \n");
						Exit=1;
					}
				}
			}
		}
		else if(2==choose)
		{
			printf("Enter ID to view your info");
			scanf("%d",&ID);
			flag=patientrecord(ID);
			if(1==flag)
			{
				printf("Thank You! :) \n");
			}
		}
		else
		{
			printf("Worng input \n");
		}
	}
	return 0;
}


u32 addnode(u8* name, u8* gender, u8 age, u32 ID)
{
	node* current=(node*)malloc(sizeof(node));
	node* last;
	node* s;
	current -> name = name;
	current -> gender = gender;
	current -> age= age;
	current -> ID= ID;
	current -> tail = NULL;
	if(NULL == head)
	{
		head = current;
	} 
	else 
	{
		last=head;
		while(NULL!= (last -> tail))
		{
			last = last -> tail;
		}
		last -> tail= current;
	}
	listlength++;
	if(listlength>0)
	{
		s=head;
		while (NULL != (s-> tail))
		{
			if( current -> ID == s -> ID)
			{
				listlength--;
				free(current);
				current= NULL;
				last -> tail = NULL;
				return 1;
			}
			s = s-> tail;
		}
	}
	
	return 2;
}
u32 editpatient(u32 ID)
{
	u8 x [10];
	u8 scan;
	u8* temp_name=(u8*)malloc(sizeof(u8));
	u8* temp_gender=(u8*)malloc(sizeof(u8));
	u8 temp_age;
	u8 i=0;
	node* last;
	last=head;
	if(0==listlength)
	{
		return 3;
	}
	else
	{
		last = head;
		while( i <= listlength )
		{	
			if(last -> ID == ID )
			{
					printf("----------------Enter 1 to edit name \n----------------Enter 2 to edit gender \n----------------Enter 3 to edit age \n");
					scanf("%d",&scan);
					 switch (scan)
					{
						case 1: 
						fflush(stdin);
						gets(temp_name);
						last -> name = temp_name;
						break;
						case 2:
						fflush(stdin);
						gets(temp_gender);
						last -> gender = temp_gender;
						break;
						case 3:
						scanf("%d",&temp_age);
						last -> age = temp_age;
						break;
						default:
						printf("wrong input");
					}
					printf("Name : %s\n", last -> name);
					printf("Gender: %s \n", last -> gender);
					printf("Age: %d \n", last -> age);
					return 1;
			}	
			last = last -> tail;
			i++;
		}
	}
	return 2;
}
u32 reserveslot (u32 ID)
{
	u8 x[10];
	node* last;
	last = head;
	u8* temp_slot;
	temp_slot=(u8*)malloc(5);
	u8 choice,i=0;
	while(i <=listlength)
	{
		if(last -> ID == ID)
		{	
			printf(" Enter 1 to 5 to resereve the slot you need \n");
			scanf("%d",&choice);
			switch (choice)
			{
				case 1:
				last -> slot = choice;
				printf("Slot 1. 2 to 2:30 PM has been reserved \n");
				break;
				case 2:
				last -> slot = choice;
				printf("Slot 2. 2:30 to 3 PM has been reserved \n");
				break;
				case 3:
				last -> slot = choice;
				printf("Slot 3. 3 to 3:30 PM has been reserved \n");
				break;
				case 4:
				last -> ID = ID;
				last -> slot = choice;
				printf("Slot 4. 4 to 4:30 PM has been reserved \n");
				break;
				case 5:
				last -> slot = choice;
				printf("Slot 5. 4:30 to 5 PM has been reserved \n");
				break;
			}
			printf("Patient with ID: %d, Resereved a slot number:  %d \n", last -> ID, last -> slot );
			head -> slot = last -> slot;
			return 1;
		}
		
		last = last -> tail;
		i++;
	}
	return 2;
}
u32 patientrecord(u32 ID)
{
	node* last;
	// last = (node*)malloc(sizeof(node));
	last = head;
	u8 i=0;
	while(i<=listlength)
	{
		if(last -> ID == ID)
		{
			printf("Name: %s \n", last -> name);
			printf("Gender: %s\n", last -> gender);
			printf("Age: %d \n", last -> age);
			printf("ID: %d \n", last -> ID);
			printf("Slot: %d \n", last -> slot);
			return 1;
		}
		last =  last-> tail;
		i++;
	}
	return 2;
}
