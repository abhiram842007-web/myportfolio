# myportfolio
hi! code code code

#include <stdio.h>
#include <stdlib.h>

struct node {
    int coeff;
    int power;
    struct node *next;
};

struct node *Poly1 = NULL;
struct node *Poly2 = NULL;
struct node *PolySum = NULL;

/* Function to Create Header Node */
void CreateHeader(struct node **Header) {
    *Header = (struct node *)malloc(sizeof(struct node));
    (*Header)->coeff = 0;
    (*Header)->power = -1;
    (*Header)->next = *Header;
}

/* Function to Insert a Term */
void Insert_Term(struct node *Header, int Coeff, int Power) {
    struct node *Temp;
    struct node *Ptr;

    Temp = (struct node *)malloc(sizeof(struct node));
    Temp->coeff = Coeff;
    Temp->power = Power;

    Ptr = Header;

    while (Ptr->next != Header && Ptr->next->power > Power) {
        Ptr = Ptr->next;
    }

    if (Ptr->next != Header && Ptr->next->power == Power) {
        Ptr->next->coeff = Ptr->next->coeff + Coeff;
        free(Temp);
    } else {
        Temp->next = Ptr->next;
        Ptr->next = Temp;
    }
}

/* Function to Create Polynomial 1 */
void Create_Poly1() {
    int Degree, i, Coeff;

    if (Poly1 != NULL) {
        free(Poly1);
        Poly1 = NULL;
    }

    CreateHeader(&Poly1);

    printf("\n\nEnter the Degree of Polynomial 1 : ");
    scanf("%d", &Degree);

    printf("\n\nEnter the Coefficients:");
    for (i = Degree; i >= 0; i--) {
        printf("\n\tCoefficient of X^%d : ", i);
        scanf("%d", &Coeff);

        if (Coeff != 0) {
            Insert_Term(Poly1, Coeff, i);
        }
    }

    printf("\n\nPolynomial 1 Created Successfully...");
}

/* Function to Create Polynomial 2 */
void Create_Poly2() {
    int Degree, i, Coeff;

    if (Poly2 != NULL) {
        free(Poly2);
        Poly2 = NULL;
    }

    CreateHeader(&Poly2);

    printf("\n\nEnter the Degree of Polynomial 2 : ");
    scanf("%d", &Degree);

    printf("\n\nEnter the Coefficients:");
    for (i = Degree; i >= 0; i--) {
        printf("\n\tCoefficient of X^%d : ", i);
        scanf("%d", &Coeff);

        if (Coeff != 0) {
            Insert_Term(Poly2, Coeff, i);
        }
    }

    printf("\n\nPolynomial 2 Created Successfully...");
}

/* Function to Display a Polynomial */
void Display(struct node *Header) {
    struct node *Ptr;
    int First;

    if (Header == NULL || Header->next == Header) {
        printf("0");
        return;
    }

    Ptr = Header->next;
    First = 1;

    while (Ptr != Header) {
        if (Ptr->coeff != 0) {
            if (First == 0) {
                if (Ptr->coeff > 0)
                    printf(" + ");
                else
                    printf(" - ");
            } else {
                if (Ptr->coeff < 0)
                    printf("-");
            }

            if (Ptr->coeff < 0)
                printf("%d", -Ptr->coeff);
            else
                printf("%d", Ptr->coeff);

            if (Ptr->power > 0) {
                printf("X");
                if (Ptr->power > 1)
                    printf("^%d", Ptr->power);
            }

            First = 0;
        }

        Ptr = Ptr->next;
    }

    if (First == 1)
        printf("0");
}

/* Function to Add Two Polynomials */
void Add_Polynomial() {
    struct node *P;
    struct node *Q;
    int SumCoeff;

    if (Poly1 == NULL || Poly2 == NULL) {
        printf("\n\nCreate Both Polynomials First...");
        return;
    }

    if (PolySum != NULL) {
        free(PolySum);
        PolySum = NULL;
    }

    CreateHeader(&PolySum);

    P = Poly1->next;
    Q = Poly2->next;

    while (P != Poly1 && Q != Poly2) {
        if (P->power == Q->power) {
            SumCoeff = P->coeff + Q->coeff;

            if (SumCoeff != 0) {
                Insert_Term(PolySum, SumCoeff, P->power);
            }

            P = P->next;
            Q = Q->next;
        } else if (P->power > Q->power) {
            Insert_Term(PolySum, P->coeff, P->power);
            P = P->next;
        } else {
            Insert_Term(PolySum, Q->coeff, Q->power);
            Q = Q->next;
        }
    }

    while (P != Poly1) {
        Insert_Term(PolySum, P->coeff, P->power);
        P = P->next;
    }

    while (Q != Poly2) {
        Insert_Term(PolySum, Q->coeff, Q->power);
        Q = Q->next;
    }

    printf("\n\nPolynomial Addition Completed Successfully...");
    printf("\n\nPOLYSUM = ");
    Display(PolySum);
}

/* Function to Display Polynomial 1 */
void Display_Poly1() {
    if (Poly1 == NULL) {
        printf("\n\nCreate Polynomial 1 First...");
    } else {
        printf("\n\nPOLY1 = ");
        Display(Poly1);
    }
}

/* Function to Display Polynomial 2 */
void Display_Poly2() {
    if (Poly2 == NULL) {
        printf("\n\nCreate Polynomial 2 First...");
    } else {
        printf("\n\nPOLY2 = ");
        Display(Poly2);
    }
}

/* Function to Display Polynomial Sum */
void Display_PolySum() {
    if (PolySum == NULL) {
        printf("\n\nPerform Polynomial Addition First...");
    } else {
        printf("\n\nPOLYSUM = ");
        Display(PolySum);
    }
}

/* Main Function */
int main() {
    int Option;

    while (1) {
        printf("\n\n------POLYNOMIAL ADDITION MENU------");
        printf("\n1 -> Create Polynomial 1");
        printf("\n2 -> Create Polynomial 2");
        printf("\n3 -> Display Polynomial 1");
        printf("\n4 -> Display Polynomial 2");
        printf("\n5 -> Add POLY1 and POLY2, Display POLYSUM");
        printf("\n6 -> Display POLYSUM");
        printf("\n7 -> Exit");
        printf("\n-----------------------------------");

        printf("\n\nEnter Your Option : ");
        scanf("%d", &Option);

        switch (Option) {
            case 1:
                Create_Poly1();
                break;

            case 2:
                Create_Poly2();
                break;

            case 3:
                Display_Poly1();
                break;

            case 4:
                Display_Poly2();
                break;

            case 5:
                Add_Polynomial();
                break;

            case 6:
                Display_PolySum();
                break;

            case 7:
                exit(0);

            default:
                printf("\n\nInvalid Option...");
        }
    }

    return 0;
}
