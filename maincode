#include <stdio.h>
#include <stdlib.h>
#define STR_LEN 40

typedef struct student{
    char name[STR_LEN];     //名字，字符串表示
    int gender;             //性别，0表示无，1表示男性，2表示女性
    int age;                //年龄，整数表示
}student;

void menu(int flag, int *num);              //用于展示系统功能并引导进入各功能
void read(student *p, int *num);            //用于读入学生数据到缓存
void init(int *num);                        //用于初始化数据库(内存),num指示运行状态
void show(student *p, int *num);            //用于打印学生数据
void showline();                            //打印一行分割线，更加美观
void open(student *p, int *num, int flag);  //用于打开本地文件读取数据
void save(student *p, int *num);            //用于保存学生数据到本地文件

student *data = NULL;   //用作数组头指针，初始化为NULL

int main(){
    system("cls");
    int num = 0;
    int flag = 0;           //flag 用于指示系统的运行状态，0表示首次打开，1表示已经打开，-1表示运行出错
    menu(flag++, &num);   //进入菜单，状态为0，同时注册状态为1
    return 0;
}

void menu(int flag, int *num){
    if(flag == 0) printf("Welcome to student management system! Please choose the function and intput the number:\n");
    else if(flag == 1) printf("Welcome back! Please choose the function and intput the number:\n");
    else if(flag == -1) printf("Error! Please Reboot the program and try again!");
    printf("\t1.Adding data\n\t2.Viewing data\n\t3.Save data\n\t4.Open data\n\t5.Quit\n\t");
    int choice = 0;
    scanf("%d", &choice);
    switch(choice){
        //选择1，则进入读入数据程序
        case 1:{
            printf("\033[0;35mPlease input the number of data which you want to input:\033[0m");
            scanf("%d", num);
            init(num);
            read(data, num);
            return;
        }
        //选择2，则进入输出数据程序
        case 2:{
            show(data, num);
        }
        //选择3，则进入保存数据程序
        case 3:{
            save(data, num);
            return;
        }
        //选择4，则进入读取数据程序
        case 4:{
            open(data, num, flag);
        }
        //选择5，则进入退出菜单程序
        case 5: exit(0);
        default: {
            printf("\033[0;31mError: Cannot identify your input!\033[0m");
            return;
        }
    }
}

void init(int *num){
    data = (student*)malloc((*num)*sizeof(student));   //为读入的数据动态开内存
    for(int i = 0; i < *num; i++){
        for(int j = 0; j < STR_LEN; j++){
            data->name[j] = 0;
        }
        data->gender = 0;
        data->age = 0;
        data++;
    }
    data -= *num;    //指针置初始位置
    read(data, num);
}

void read(student *p, int *num){
    for(int i = 0; i < *num; i++){
        printf("\tStudent Name:");
        scanf("%s", p->name);
        printf("\tStudent Gender(1-Male;2-Female):");
        scanf("%d",&(p->gender));
        printf("\tStudent Age:");
        scanf("%d",&(p->age));
        showline();
        printf("\033[0;33m%d students has been added, remaining %d students.\033[0m\n", i+1, *num-i);
        showline();
        p++;
    }
    showline();
    printf("\033[0;32mAll students's data has benn added to the system.now back to main system\033[0m\n");
    showline();
    menu(1, num);    //返回菜单，状态为1
}

void show(student *p, int *num){
    for(int i = 0; i < *num; i++){
        showline();
        printf("\tName: %s\n",p->name);
        if(p->gender == 1) printf("\tGender: Male\n");
        else if(p->gender == 2) printf("\tGender: Female\n");
        else printf("\tGender: Unidentified\n");
        printf("\tAge: %d\n", p->age);
        p++;
    }
    showline();
    printf("\033[0;32mAll data has benn printed on the screen! Now back to menu...\033[0m\n");
    showline();
    menu(1, num);
}

void save(student *p, int *num){
    FILE *fp = fopen("studentdata", "w+");
    if(fp){
        fwrite(p, sizeof(student), *num, fp);
        fclose(fp);
        printf("\033[42;37m Save Success!\033[0m\n");
    }
    else printf("\033[41;37m Save Failed!\033[0m\n");
    menu(1, num);
}

void open(student *p, int *num, int flag){
    *num = 0;
    FILE *fp = fopen("studentdata", "r");
    if(fp){
        fseek(fp, 0L, SEEK_END);     //移动指针到文件尾
        long size = ftell(fp);              //此时fp的偏移字节数即为文件总数据大小
        *num = size/(sizeof(student));      //通过除法计算数据块数量
        fseek(fp, 0L, SEEK_SET);     //移动指针到文件头

        if(flag == 1) free(data);           //如果运行状态为1，应当释放内存重新申请
        data = (student *)malloc((*num)*sizeof(student));
        p = data;

        fread(data, sizeof(student), *num, fp);

        showline();
        printf("\033[0;34mA total of %d data were successfully read.\033[0m\n", *num);
        showline();
    }
    else printf("\033[41;37mRead Failed!\033[0m\n");
    menu(1, num);
}

void showline(){
    for(int i = 0; i < 50; i++){
        printf("*-");
    }
    printf("*\n");
}
