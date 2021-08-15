# cvaish
first
commit
#include<stdio.h>
typedef struct account{
	char name[30];
	int accNumber,count;
	float balance;
}account_t ;
typedef struct bank {
   char bankname[30];
   char branch[30];
   account_t * a[5];	
}bank_t;
FILE * in;
FILE * out;
bank_t * a1;
bank_t * a2;
bank_t * temp;
void accept_account_info(account_t *ptraccount);
void print_account_info(account_t * ptraccount);
void accept_bank_info(bank_t * ptrbank);
void print_bank_info(bank_t * ptrbank);
int create_account(bank_t * ptrbank);
int delete_account(bank_t * ptrbank,int accNumber);
float deposit(bank_t * ptrbank,int accNumber,float amount);
float withdraw (bank_t * ptrbank,int accNumber ,float amount);
int fund_transfer(bank_t * ptrbank ,int srcaccNumber,int destaccNumber, float amount);
void write_record(bank_t * ptrbank);
void read_record(bank_t * ptrbank);
int ministatement(bank_t * ptrbank,int accNumber);
void main()
{
	int i,c;
    int choi,accno1,accno2,checkin;
	float check,amoun;
	a1 =(bank_t*)malloc(1*sizeof(bank_t));
		for ( i =0,c=1000;i<5;i++,c++)
	{
		a1->a[i]= (account_t*)calloc(1,sizeof(account_t));
	    a1->a[i]->accNumber = c;
    }
    	a2 =(bank_t*)malloc(1*sizeof(bank_t));
		for ( i =0;i<5;i++)
	{
		a2->a[i]= (account_t*)calloc(1,sizeof(account_t));
    }
   	while(1){
    printf("\n press number according to choice");
	printf(" \n 1. create new account ");
	printf(" \n 2. deposit amount ");
	printf(" \n 3. withdraw amount ");
	printf(" \n 4. transfer fund ");
	printf(" \n 5. delete account ");
	printf(" \n 6. save account ");
	printf(" \n 7. load account ");
	printf("\n 8. ministatement");
	scanf(" %d",&choi);
	switch(choi)
	{
		case 0 :
                 printf("\ndemo");
                 print_bank_info(a1);
				 break;               
		case 1 : 
		         printf("\n create new account");
				 accno1 = create_account(a1);
				 printf("\nyour account no : %d",accno1);
				 getc(stdin); 
		  break;
		case 2 : 
		          printf("\n deposit amount");
                  printf("\n enter account number");
                  scanf("%d",&accno1);
                  printf("\n enter amount which you want to deposit ");
                  scanf("%f",&amoun);
		          check = deposit(a1,accno1,amoun);
		          if (check != 0)
		          printf("\nsuccessfully deposited\n your bankbalance : %f",check);
			break;
		case 3 :
			     printf(" \n withdraw amount ");
			     printf("\n enter your accnumber: ");
			     scanf("%d",&accno1);
			     printf("\n enter amount :");
			     scanf("%f",&amoun);
			     check = withdraw (a1,accno1,amoun);
                 if (check!=0)
				 printf("\n successfully withdraw your remaining amount : %f ",check); 
			break;
		case 4 :
		         printf("transfer amount");
				 printf("\n sender account number: ");
				 scanf("%d",&accno1);
				 printf("\n receiver account number :"); 	     
			     scanf("%d",&accno2);
			     printf("\n enter amount to transfer :");
			     scanf("%f",&amoun);
			     checkin = fund_transfer(a1,accno1,accno2,amoun);
			     if(checkin)
			     printf("\n tranfer your amount sucessfully");
			break;
  	        case 5 :
                  printf("\n enter account number:");
				  scanf("%d",&accno1);			
                  checkin = delete_account(a1,accno1);
                  if (checkin)
                  printf("\n successfully deleted ");
   		        	break;
   		
        	case 6 :
			       printf("\n  write file ");
			       write_record(a1);
			       break;
		    case 7 :
 			      printf("\n read  file ");
			      read_record(a1);
		       	break;
	    case 8:
	    	     printf("\n to check account ministatement : ");
	    	     printf("\n enter your accountnumber:");
	    	     scanf("%d",&accno1);
	    	     ministatement(a1,accno1);
	    	     break ;
		default :
		          printf(" invalid ");
		          exit(0);
	 }
  }
	 getc(stdin);
}
void accept_account_info(account_t * ptraccount)
   {
	printf("enter account details");
	printf("\n enter name ");
	scanf("%s",& ptraccount->name);
	printf(" balance : %f",ptraccount->balance);
	}
void print_account_info(account_t * ptraccount)
    {
	printf("name : %s",ptraccount->name);
	printf("\naccnumber : %d", ptraccount->accNumber);
	printf("\n balance : %d",ptraccount->balance);	
	}	
void accept_bank_info(bank_t * ptrbank)
  {
	int i,c;
	printf(" enter  your bank details");
	printf("\n bank name : ");
	scanf("%s",&ptrbank->bankname);
	printf("\n branch name :");
	scanf("%s",&ptrbank->branch);
    printf(" name : ");
    scanf("%s",ptrbank->a[0]->name);
    printf(" balance : %f ",ptrbank->a[0]->balance);
  }
void print_bank_info(bank_t * ptrbank)
{
	int i;
	printf("\n bank name : %s",ptrbank->bankname);
	printf("\n branch name : %s",ptrbank->branch);
	for (i=0;i<5;i++)
	{
		printf(" \n name : %s",ptrbank->a[i]->name);
		printf(" \n accnumber: %d",ptrbank->a[i]->accNumber);
		printf("\n  accbalance :%f " ,ptrbank->a[i]->balance);
	}
}
int create_account(bank_t * ptrbank){
     int i,c;
     printf("\n enter bank name: ");
     scanf(" %[^\n]s",&ptrbank->bankname);
     printf("enter bank branch :");
     scanf(" %[^\n]s",&ptrbank->branch);
      for(i=0;i<5;i++){
    	if(ptrbank->a[i]->count == 0){
    	  printf("enter name :");
		  scanf(" %[^\n]s",&ptrbank->a[i]->name);
		  printf("balance : %d ",ptrbank->a[i]->balance);
		  ptrbank->a[i]->count = 1;	
          return ptrbank->a[i]->accNumber;
         }
      }
 }
float deposit(bank_t * ptrbank ,int accNumber,float amount)	
{
  int i,c=0;
  for (i=0;i<5;i++)
  {
  	if (accNumber == ptrbank->a[i]->accNumber && ptrbank->a[i]->count == 1)
	    {
  		    ptrbank->a[i]->balance +=amount;
  	         c++;
		  	return ptrbank->a[i]->balance;
        }
  }
    if (c ==0){
    	printf("you enter invalid account number");
    	return 0;
	}
}
float withdraw(bank_t * ptrbank,int accNumber ,float amount)
{
    int i ,c=0;
  for (i=0;i<5;i++)
	 {  
	    if (accNumber == ptrbank->a[i]->accNumber && ptrbank->a[i]->count == 1)
	       {
	    	if (ptrbank->a[i]->balance != 0)
	    	ptrbank->a[i]->balance -= amount;
	        else
	        printf(" your account are empty ");
	        c++;
	        return ptrbank->a[i]->balance ;
	    	}      
     }	 
   if (c==0)
	{
		printf("invalid account number ");
		return 0;
	}
}
int fund_transfer(bank_t * ptrbank ,int srcaccNumber,int destaccNumber, float amount)
{
	 int i ,c=0;
	if (srcaccNumber == destaccNumber)
	{
		printf("  invalid case : you print same account number");
		return 0;
	}
	for(i=0;i<5;i++)
	{
		if (ptrbank->a[i]->accNumber == srcaccNumber && ptrbank->a[i]->count ==1)
		{   
		    if (ptrbank->a[i]->balance == 0)
		    {
		    	printf("\nyour account is zero balance account");
		    	return 0;
			}
			if(ptrbank->a[i]->balance < amount)
			{
				printf("\n not enough balance ");
				return 0;
			}
            c++;
  		}
  		if (ptrbank->a[i]->accNumber== destaccNumber && ptrbank->a[i]->count ==1)
		{
			c++;	
		}
		
	}
	if(c!=2)
	{
		printf(" invalid your account numbers are not exist ");
		return 0;
	}
	else if(c==2)
	{
		for(i=0;i<5;i++){
		if (ptrbank->a[i]->accNumber == srcaccNumber && ptrbank->a[i]->count ==1)
				ptrbank->a[i]->balance -=amount;
	   	if (ptrbank->a[i]->accNumber== destaccNumber && ptrbank->a[i]->count ==1)
	   	        	ptrbank ->a[i]->balance +=amount;
	   }
	}
    return 1;
}
int delete_account(bank_t * ptrbank,int accNumber)
{
	int i,j,c1=1 ;
	char ch[30]=" ";
	for(i=0;i<5;i++)
	{
		if (ptrbank->a[i]->accNumber == accNumber)
          {
          	c1=0;
             if (ptrbank->a[i]->name != 0)
             {
             	for(j=0;j<30;j++)
				 {
             	ptrbank->a[i]->name[j] =ch[j];
                 }
             	ptrbank->a[i]->balance =0;
             	ptrbank->a[i]->count =0;
             	return 1;
			 }
             else
			 {
             printf("unregistered account");
             return 0;
             }
  		  }
    
    }
    if(c1)
    {
    printf("\n does not match");
    return 0;
	}
 }
void write_record(bank_t * ptrbank)
{
      int i;
      out = fopen("accounts.txt","a");
      if(out == NULL)
      {
      	printf("invalid");
	  }
	  else 
	  {
          fprintf(out,"%10s\n",ptrbank->bankname);
          fprintf(out,"%10s\n",ptrbank->branch);
         for (i =0;i<5;i++)
            { 
               fprintf(out,"%10s \t",ptrbank->a[i]->name);
	    	   fprintf(out,"%d \t",ptrbank->a[i]->accNumber); 
	    	   fprintf(out,"%f \n",ptrbank->a[i]->balance);      	
            }
       }
    fclose(out);  
}
void read_record(bank_t * ptrbank)
{   
    int i;
     in =fopen("accounts.txt","r");
	if(in == NULL)
	{ 
	printf("invalid");
	}
	else
	{
	fscanf(in," %[^\n]s",&a2->bankname);
	fscanf(in," %[^\n]s",&a2->branch);
	for (i=0;i<5;i++)
	{
		fscanf(in," %[^\t]s",&a2->a[i]->name);
	    fscanf(in,"%d",&a2->a[i]->accNumber);
	    fscanf(in,"%f",&a2->a[i]->balance);
	} 
	printf("\n%s",a2->bankname);
	printf("\n%s\n",a2->branch);
	for (i=0;i<5;i++)
	{
	    printf("%20s \t",a2->a[i]->name);
	    printf("       %d \t",a2->a[i]->accNumber);
	    printf("       %f \n",a2->a[i]->balance);
	}  
    }
   fclose(in);
}	  
int ministatement(bank_t * ptrbank,int accNumber)
{
	int i,c =0;
	for (i=0;i<5;i++)
	{
	     if (ptrbank->a[i]->accNumber == accNumber && ptrbank->a[i]->count == 1)
	      {
      	  printf("\n name : %s",ptrbank->a[i]->name);
		  printf("\n account number :  %d", ptrbank->a[i]->accNumber);
		  printf("\n balance : %f", ptrbank->a[i]->balance);	
		  c++;
          return 1;
          }
    }
    if(c==0)
    {
    	printf(" you enter invalid account number ");
	      return 0;
	}
}
