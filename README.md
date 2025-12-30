#include <stdio.h>
#include <stdlib.h>
#include <string.h>
#include <time.h>

// Project - Parking Managment System
// Table T1 -> 2 Wheeler vehicle
//Table T2 -> 4 Wheeler vehicles


//Table for Tower 1

typedef struct s1 {
    int sno;
    char date[20];
    char tower_no[5];
    int slot_no;
    char status[20];
    char carno[20];
    char contact_no[20];
    int enhour;
    int enmin;
} s1;

//Table for Tower 2

typedef struct s2 {
    int sno;
    char date[20];
    char tower_no[5];
    int slot_no;
    char status[20];
    char carno[20];
    char contact_no[20];
    int enhour;
    int enmin;
} s2;


void struct1(s1 T1[]);
void struct2(s2 T2[]);

// Functions defination

void save_T1(s1 T1[], int count) {
    FILE *fp = fopen("dat1.txt", "w");
    if (fp == NULL) {
        printf("Error opening file \n");
        return;
    }

    for (int i = 0; i < count; i++) {


        fprintf(fp, "%d %s %s %d %s %s %s %d %d\n",
                T1[i].sno,
                T1[i].date,
                T1[i].tower_no,
                T1[i].slot_no,
                T1[i].status,
                T1[i].carno,
                T1[i].contact_no,
                T1[i].enhour, // entry hour
                T1[i].enmin);
    }

    fclose(fp);
    printf("T1 saved successfully to dat1.txt\n");
}

void save_T2(s2 T2[], int count) {
    FILE *fp = fopen("dat2.txt", "w");
    if (fp == NULL) {
        printf("Error opening file \n");
        return;
    }


    for (int i = 0; i < count; i++)
        {
        fprintf(fp, "%d %s %s %d %s %s %s %d %d\n",
                T2[i].sno,
                T2[i].date,
                T2[i].tower_no,
                T2[i].slot_no,
                T2[i].status,
                T2[i].carno,
                T2[i].contact_no,
                T2[i].enhour,
                T2[i].enmin);
    }

    fclose(fp);

    printf("T2 saved successfully to dat2.txt \n");
}
// Function for EXIT TIME CALCULATION & SLIP GENERATION

//EDIT
void calc_rev(int checkin_h, int checkin_m, int checkout_h, int checkout_m) {

    int entry_minutes = checkin_h * 60 + checkin_m;
    int exit_minutes  = checkout_h * 60 + checkout_m;

    int duration = exit_minutes - entry_minutes;

    if (duration <= 0) {
        printf("Invalid time input\n");
        return;
    }

    int payment = 0;
    if (duration <= 60)
        payment = 50;
    else if (duration <= 180)
        payment = 80;
    else
        payment = 80 + (duration - 180) * 2;

    //Final SLIP GENERATED

    printf("\n------ PARKING BILL ------\n");
    printf("Entry Time : %02d %02d\n", checkin_h, checkin_m);
    printf("Exit Time  : %02d %02d\n", checkout_h, checkout_m);
    printf("Duration   : %d minutes\n", duration);
    printf("Payment    : %d\n", payment);
    printf("---------------------------\n");
}


int main() {
    s1 T1[100];
    s2 T2[100];
    FILE *fptr;
    int a = 0;
    int b = 0;


    // initialize arrays to safe defaults to avoid garbage
    for (int i = 0; i < 100; i++) {
        T1[i].sno = i + 1;
        strcpy(T1[i].date, "NA");
        strcpy(T1[i].tower_no, "T1");
        T1[i].slot_no = i + 1;
        strcpy(T1[i].status, "EMPTY");
        strcpy(T1[i].carno, "NA");
        strcpy(T1[i].contact_no, "NA");
        T1[i].enhour = 0;
        T1[i].enmin = 0;

        T2[i].sno = i + 1;
        strcpy(T2[i].date, "NA");
        strcpy(T2[i].tower_no, "T2");
        T2[i].slot_no = i + 1;
        strcpy(T2[i].status, "EMPTY");
        strcpy(T2[i].carno, "NA");
        strcpy(T2[i].contact_no, "NA");
        T2[i].enhour = 0;
        T2[i].enmin = 0;
    }

    fptr = fopen("dat1.txt", "r");
    if (fptr != NULL)
        {
        while (fscanf(fptr, "%d %19s %4s %d %19s %19s %19s %d %d",
                      &T1[a].sno,
                      T1[a].date,
                      T1[a].tower_no,
                      &T1[a].slot_no,
                      T1[a].status,
                      T1[a].carno,
                      T1[a].contact_no,
                      &T1[a].enhour,
                      &T1[a].enmin ) == 9) {
            a++;
            if (a >= 100) break;
        }
        fclose(fptr);
    } else {


    }

    fptr = fopen("dat2.txt", "r");

    if (fptr != NULL) {
        while (fscanf(fptr, "%d %19s %4s %d %19s %19s %19s %d %d",
                      &T2[b].sno,
                      T2[b].date,
                      T2[b].tower_no,
                      &T2[b].slot_no,
                      T2[b].status,
                      T2[b].carno,
                      T2[b].contact_no,
                      &T2[b].enhour,
                      &T2[b].enmin) == 9) {
            b++;
            if (b >= 100) break;
        }
        fclose(fptr);
    } else {


    }

    srand((unsigned int)time(NULL));

    //Main Menue
    int choice;
    int j = 1;
    while (j == 1) {
        printf("\n");
        printf("\n");

        printf(" \t ------------------------  PARKING MANAGEMENT SYSTEM  ------------------------------\n");
        printf("\n");
        printf("1. Admin Mode\n");
        printf("2. Manual Parking System\n");
        printf("3. Automatic Parking System\n");
        printf("4. Revenue Collection Report\n");
        printf("5. Exit \n");
        printf("\n");
        printf("Enter your choice: ");
        if (scanf("%d", &choice) != 1) {
            while (getchar() != '\n');
            continue;
        }

        switch (choice) {

        case 1:
        {
            char pass[50];
            printf("\n");
            printf("ENTER PASSWORD   : ");// password = admin
            scanf("%s", pass);

            if (strcmp(pass, "admin") == 0)
            {
                struct1(T1);
                struct2(T2);
            }
            else
            {
                printf("\n");
                printf("wrong password\n");
                printf("Try Again \n");
                printf("\n");
            }

            break;
        }

        case 2:
        {

            int t, slot, aslot;
            while (1) {
                printf("Enter 2 wheeler or 4 wheeler (enter 2 or 4): ");
                if (scanf("%d", &t) != 1) { while (getchar() != '\n'); break; }

                if (t == 2) {
                    for (int i = 0; i < 99; i++) {
                        if (strcmp(T1[i].status, "EMPTY") == 0) {
                            printf("Sr No : %d || Date: %s || TOWER_NO: %s || SLOT_NO.: %d || STATUS: %s || CAR_NO: %s || CONTACT_NO: %s || Entry Hour : %d || Entry Min : %d\n",
                                   T1[i].sno, T1[i].date, T1[i].tower_no, T1[i].slot_no, T1[i].status, T1[i].carno, T1[i].contact_no, T1[i].enhour, T1[i].enmin);
                        }
                    }
                    printf("Enter Slot no: ");
                    scanf("%d", &slot);

                    for (int j2 = 0; j2 < 99; j2++) {
                        if (T1[j2].slot_no == slot) {
                            aslot = slot - 1;
                            break;
                        }

                    }

                    //Customer details input for T1

                    printf("Enter the following details:\n");

                    printf("ENTER THE DATE(DD-MM-YYYY): ");
                    scanf("%s", T1[aslot].date);

                    printf("Enter contact no: ");
                    scanf("%s", T1[aslot].contact_no);

                    strcpy(T1[aslot].status, "FULL");

                    printf("Enter car no: ");
                    scanf("%s", T1[aslot].carno);

                    printf("Enter entry hour: \n");
                    scanf("%d", &T1[aslot].enhour);

                    printf("Enter entry min: \n");
                    scanf("%d", &T1[aslot].enmin);

                    save_T1(T1, 99);

                    printf("Slot %d Updated Successfully \n\n", slot);
                }

                if (t == 4)
                {
                    for (int i = 0; i < 99; i++) {
                        if (strcmp(T2[i].status, "EMPTY") == 0) {
                            printf("Sr No : %d || Date: %s || TOWER_NO: %s || SLOT_NO.: %d || STATUS: %s || CAR_NO: %s || CONTACT_NO: %s || Entry Hour : %d || Entry Min : %d \n",
                                   T2[i].sno, T2[i].date, T2[i].tower_no, T2[i].slot_no, T2[i].status, T2[i].carno, T2[i].contact_no, T2[i].enhour, T2[i].enmin);
                        }
                    }
                    printf("Enter Slot no: ");
                    scanf("%d", &slot);

                    for (int j2 = 0; j2 < 99; j2++) {
                        if (T2[j2].slot_no == slot) {
                            aslot = slot - 1;
                            break;
                        }

                    }

                    //Customer details input for T2

                    printf("Enter the following details:\n");

                    printf("Enter date: \n");
                    scanf("%s", T2[aslot].date);

                    printf("Enter contact no: \n");
                    scanf("%s", T2[aslot].contact_no);

                    strcpy(T2[aslot].status, "FULL");

                    printf("Enter car no: ");
                    scanf("%s", T2[aslot].carno);

                    printf("Enter entry hour : \n");
                    scanf("%d", &T2[aslot].enhour);

                    printf("Enter entry min : \n");
                    scanf("%d", &T2[aslot].enmin);


                    save_T2(T2, 99);

                    printf("Slot %d Updated Successfully \n\n", slot);
                }

                {
                    int Ch;
                    printf("Want to continue:\n ");
                    printf("Enter 1 for yes \n");
                    printf("Enter 2 for no \n");
                    scanf("%d", &Ch);

                    if (Ch == 2) {
                        break;
                    }
                }
            }
            break;
        }
        case 3:{
            while(1){
                int sensor = rand() % 2;
                if (sensor == 1){
                    printf("\n");
                    printf("Vehicle detected at the entry gate \n");
                    printf("\n");


                    int t,slot,aslot;
                    while (1)
                    {
                        printf("Enter 2 wheeler or 4 wheeler (2/4): ");
                        if (scanf("%d", &t) != 1) { while (getchar() != '\n'); break; }


                        if (t == 2) {
                            for (int i = 0; i < 99; i++) {
                                if (strcmp(T1[i].status, "EMPTY") == 0) {
                                    printf("Sr No : %d || Date: %s || TOWER_NO: %s || SLOT_NO.: %d || STATUS: %s || CAR_NO: %s || CONTACT_NO: %s || Entry Hour : %d || Entry Min : %d \n",
                                           T1[i].sno, T1[i].date, T1[i].tower_no, T1[i].slot_no, T1[i].status, T1[i].carno, T1[i].contact_no, T1[i].enhour, T1[i].enmin);
                                }
                            }
                            printf("Enter Slot no: ");
                            scanf("%d", &slot);

                            for (int j2 = 0; j2 < 99; j2++) {
                                if (T1[j2].slot_no == slot) {
                                    aslot = slot - 1;
                                    break;
                                }
                            }


                            printf("Enter the following details:\n");

                            printf("ENTER THE DATE: ");
                            scanf("%s", T1[aslot].date);

                            printf("Enter contact no: ");
                            scanf("%s", T1[aslot].contact_no);


                            strcpy(T1[aslot].status, "FULL");

                            printf("Enter car no: ");
                            scanf("%s", T1[aslot].carno);


                            printf("Enter entry hour: \n");
                            scanf("%d", &T1[aslot].enhour);

                            printf("Enter entry min: \n");
                            scanf("%d", &T1[aslot].enmin);


                            save_T1(T1, 99);
                            printf("Slot %d Updated Successfully\n\n", slot);
                        }


                        if (t == 4)
                        {
                            for (int i = 0; i < 99; i++) {
                                if (strcmp(T2[i].status, "EMPTY") == 0) {
                                    printf("Sr No : %d || Date: %s || TOWER_NO: %s || SLOT_NO.: %d || STATUS: %s || CAR_NO: %s || CONTACT_NO: %s || Entry Hour : %d || Entry Min : %d \n",
                                           T2[i].sno, T2[i].date, T2[i].tower_no, T2[i].slot_no, T2[i].status, T2[i].carno, T2[i].contact_no, T2[i].enhour, T2[i].enmin);
                                }
                            }
                            printf("Enter Slot no: ");
                            scanf("%d", &slot);

                            for (int j2 = 0; j2 < 99; j2++) {
                                if (T2[j2].slot_no == slot) {
                                    aslot = slot - 1;
                                    break;
                                }

                            }


                            printf("Enter the following details:\n");

                            printf("ENTER THE DATE: ");
                            scanf("%s", T2[aslot].date);

                            printf("Enter contact no: ");
                            scanf("%s", T2[aslot].contact_no);

                            strcpy(T2[aslot].status, "FULL");

                            printf("Enter car no: ");
                            scanf("%s", T2[aslot].carno);


                            printf("Enter entry hour: \n");
                            scanf("%d", &T2[aslot].enhour);

                            printf("Enter entry min: \n");
                            scanf("%d", &T2[aslot].enmin);

                            save_T2(T2, 99);

                            printf("Slot %d Updated Successfully\n\n", slot);
                        }

                        break; /* break inner while after one vehicle processed */
                    }
                }

                else{
                    printf("\n");
                    printf("SORRY \n");
                    printf("No vehicle detected \n");
                    printf("\n");
                }
                int Ch;
                printf("Want to continue:\n ");
                printf("Enter 1 for yes \n");
                printf("Enter 2 for no \n");
                scanf("%d", &Ch);
                if (Ch == 2) {
                    break;
                }
            }
            break;
        }
        case 4:
        {
            //exit

            int x, eslot = -1;
            int exhour, exmin;
            char arr[30];
            printf("Enter 2 or 4 wheeler (2/4): ");
            scanf("%d",&x);

            if(x==2)
            {
                printf("Enter vehicle number: ");
                scanf("%s", arr);
                for(int i=0;i<99;i++){
                    if(strcmp(T1[i].carno, arr)==0){
                        eslot = i;
                        break;
                    }
                }
                if (eslot == -1) {
                    printf("Vehicle not found in T1\n");
                    break;
                }
                printf("Enter exit hour: \n");
                scanf("%d", &exhour);

                printf("Enter exit min: \n");
                scanf("%d", &exmin);


                calc_rev(T1[eslot].enhour,T1[eslot].enmin, exhour,exmin);

                strcpy(T1[eslot].contact_no, "NA");
                strcpy(T1[eslot].carno, "NA");
                strcpy(T1[eslot].date, "NA");
                strcpy(T1[eslot].status, "EMPTY");
                T1[eslot].enhour = 0;
                T1[eslot].enmin = 0;

                save_T1(T1, 99);

                break;

            }

            if (x==4)
            {
                printf("Enter vehicle number: ");
                scanf("%s", arr);
                for(int i=0;i<99;i++){
                    if(strcmp(T2[i].carno, arr)==0){
                        eslot = i;
                        break;
                    }
                }
                if (eslot == -1) {
                    printf("Vehicle not found in T2\n");
                    break;
                }
                printf("Enter exit hour: \n");
                scanf("%d", &exhour);

                printf("Enter exit min: \n");
                scanf("%d", &exmin);


                calc_rev(T2[eslot].enhour,T2[eslot].enmin , exhour, exmin);
                strcpy(T2[eslot].contact_no, "NA");
                strcpy(T2[eslot].carno, "NA");
                strcpy(T2[eslot].date, "NA");
                strcpy(T2[eslot].status, "EMPTY");
                T2[eslot].enhour = 0;
                T2[eslot].enmin = 0;

                save_T2(T2, 99);

                break;
            }


            break;
        }
        case 5:
        {
            printf("------------------------------------------------------------------------------------------------------------------\n");
            printf("THANKYOU FOR USING OUR PARKING MANAGMENT SYSTEM\n");
            printf("------------------------------------------------------------------------------------------------------------------\n");

            return 0;
            break;

        }
        default:
            printf("Invalid choice\n");
            break;
        }
    }
    return 0;
}

/* display functions */
void struct1(s1 T1[]) {
    printf("\n\n");
    printf("-------------------------------------------TABLE FOR T1------------------------------------------------\n");
    printf("\n");
    for(int i=0;i<=99;i++){
        printf("Sr No : %d || Date: %s || TOWER_NO: %s || SLOT_NO.: %d || STATUS: %s || CAR_NO: %s || CONTACT_NO: %s || Entry Hour : %d || Entry Min : %d\n",
               T1[i].sno, T1[i].date, T1[i].tower_no, T1[i].slot_no, T1[i].status, T1[i].carno, T1[i].contact_no, T1[i].enhour, T1[i].enmin);
        printf("\n");
    }
    printf("--------------------------------------------------------------------------------------------------------------\n");
}

void struct2(s2 T2[]) {
    printf("\n\n");
    printf("-------------------------------------------TABLE FOR T2----------------------------------------------------\n");
    printf("\n");
    for(int i=0;i<=99;i++){
        printf("Sr No : %d || Date: %s || TOWER_NO: %s || SLOT_NO.: %d || STATUS: %s || CAR_NO: %s || CONTACT_NO: %s || Entry Hour : %d || Entry Min : %d \n",
               T2[i].sno, T2[i].date, T2[i].tower_no, T2[i].slot_no, T2[i].status, T2[i].carno, T2[i].contact_no, T2[i].enhour, T2[i].enmin);
        printf("\n");
    }
    printf("-----------------------------------------------------------------------------------------------------------------\n");
}
# Automated-parking-database-management-system
