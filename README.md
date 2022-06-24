#include <GL/glut.h>
#include <math.h>
#include <iostream>
using namespace std;

#define FPS 120
#define TO_RADIANS 3.14/180.0

//  Anggota Kelompok X
//  Mohammad Dicky Alfarizi (672020082)
//  Yoga Candra Adi Pratama (672020090)
//  Restu Indah Pratiwi (672020097)
//  Muhammad Wisnu Nugroho (672020098)
//  Liza Widyarini (672020170)

const int width = 1280;
const int height = 720;
int i;
float sudut;
double x_geser, y_geser, z_geser;

float pitch = 0.0, yaw = 0.0;
float camX = 0.0, camZ = 0.0, terbang = 25.0;

void display();
void reshape(int w, int h);
void timer(int);
void passive_motion(int, int);
void camera();
void keyboard(unsigned char key, int x, int y);
void keyboard_up(unsigned char key, int x, int y);

struct Motion {
    bool Forward, Backward, Left, Right, Naik, Turun;
};
Motion motion = { false,false,false,false,false,false };

void init() {
    glClearColor(0.529, 0.807, 0.921, 0.0);
    glutSetCursor(GLUT_CURSOR_NONE);
    glEnable(GL_DEPTH_TEST);
    glEnable(GL_BLEND);
    glBlendFunc(GL_SRC_ALPHA, GL_ONE_MINUS_SRC_ALPHA);
    glDepthFunc(GL_LEQUAL);
    glutWarpPointer(width / 2, height / 2);
}

void ground() {
    glBegin(GL_QUADS);
    glColor3f(0.3f, 0.3f, 0.3f);
    glVertex3f(-1000.0, 0, -1000.0);

    glColor3f(0.6f, 0.6f, 0.6f);
    glVertex3f(1000.0, 0, -1000.0);

    glColor3f(0.6f, 0.6f, 0.6f);
    glVertex3f(1000.0, 0, 1000.0);

    glColor3f(0.3f, 0.3f, 0.3f);
    glVertex3f(-1000.0, 0, 1000.0);
    glEnd();
}


void lantai() {
        //dasar1
        //depan 
        glPushMatrix();
        glTranslatef(0, 0, -300);
        glColor3f(0.33f, 0.33f, 0.33f);
        glBegin(GL_POLYGON);
        glVertex3f(-500, 200, -200);
        glVertex3f(-650, 0, -200);
        glVertex3f(650, 0, -200);
        glVertex3f(500, 200, -200);
        glEnd();
        glPopMatrix();

        //belakang
        glPushMatrix();
        glColor3f(0.33f, 0.33f, 0.33f);
        glBegin(GL_POLYGON);
        glVertex3f(-500, 200, 500);
        glVertex3f(-650, 0, 500);
        glVertex3f(650, 0, 500);
        glVertex3f(500, 200, 500);
        glEnd();
        glPopMatrix();

        //kiri
        glPushMatrix();
        glColor3f(0.33f, 0.33f, 0.33f);
        glBegin(GL_POLYGON);
        glVertex3f(-500, 200, -500);
        glVertex3f(-650, 0, -500);
        glVertex3f(-650, 0, 500);
        glVertex3f(-500, 200, 500);
        glEnd();
        glPopMatrix();

        //KANAN
        glPushMatrix();
        glColor3f(0.33f, 0.33f, 0.33f);
        glBegin(GL_POLYGON);
        glVertex3f(500, 200, -500);
        glVertex3f(650, 0, -500);
        glVertex3f(650, 0, 500);
        glVertex3f(500, 200, 500);
        glEnd();
        glPopMatrix();

        //atas
        glPushMatrix();
        glColor3f(0.33f, 0.33f, 0.33f);
        glBegin(GL_POLYGON);
        glVertex3f(-500, 200, 500);
        glVertex3f(-500, 200, -500);
        glVertex3f(500, 200, -500);
        glVertex3f(500, 200, 500);
        glEnd();
        glPopMatrix();
}

void lantai1() {
    //depan
    glPushMatrix();
    glTranslatef(0, 0, 224);
    glColor3f(0.95f, 0.95f, 0.95f);
    glBegin(GL_POLYGON);
    glVertex3f(-400, 50, 100);
    glVertex3f(-400, 300, 100);
    glVertex3f(400, 300, 100);
    glVertex3f(400, 50, 100);
    glEnd();
    glPopMatrix();

    //belakang
    glPushMatrix();
    glColor3f(0.95f, 0.95f, 0.95f);
    glBegin(GL_POLYGON);
    glVertex3f(-400, 50, -450);
    glVertex3f(-400, 300, -450);
    glVertex3f(400, 300, -450);
    glVertex3f(400, 50, -450);
    glEnd();
    glPopMatrix();

    //kiri
    glPushMatrix();
    glColor3f(0.95f, 0.95f, 0.95f);
    glBegin(GL_POLYGON);
    glVertex3f(-400, 50, -450);
    glVertex3f(-400, 300, -450);
    glVertex3f(-400, 300, 325);
    glVertex3f(-400, 50, 325);
    glEnd();
    glPopMatrix();

    //KANAN
    glPushMatrix();
    glColor3f(0.95f, 0.95f, 0.95f);
    glBegin(GL_POLYGON);
    glVertex3f(400, 50, -450);
    glVertex3f(400, 300, -450);
    glVertex3f(400, 300, 325);
    glVertex3f(400, 50, 325);
    glEnd();
    glPopMatrix();

    //atas
    glPushMatrix();
    glColor3f(0.95f, 0.95f, 0.95f);
    glBegin(GL_POLYGON);
    glVertex3f(-400, 300, -450);
    glVertex3f(400, 300, -450);
    glVertex3f(400, 300, 325);
    glVertex3f(-400, 300, 325);
    glEnd();
    glPopMatrix();
}

void atap() {
    //depan 
    glPushMatrix();
    glTranslatef(0, 500, 0);
    glColor3f(1.0f, 0.51f, 0.1f);
    glBegin(GL_POLYGON);
    glVertex3f(-400, 50, 500);
    glVertex3f(-500, 100, 600);
    glVertex3f(500, 100, 600);
    glVertex3f(400, 50, 500);
    glEnd();
    glPopMatrix();

    //kiri 
    glPushMatrix();
    glTranslatef(0, 500, 119);
    glColor3f(1.0f, 0.51f, 0.1f);
    glBegin(GL_POLYGON);
    glVertex3f(-400, 50, -389);
    glVertex3f(-500, 100, -480);
    glVertex3f(-500, 100, 480);
    glVertex3f(-400, 50, 389);
    glEnd();
    glPopMatrix();

    //KANAN 
    glPushMatrix();
    glTranslatef(-1, 500, 0);
    glColor3f(1.0f, 0.51f, 0.1f);
    glBegin(GL_POLYGON);
    glVertex3f(400, 50, -272);
    glVertex3f(500, 100, -400);
    glVertex3f(500, 100, 600);
    glVertex3f(400, 50, 500); 
    glEnd();
    glPopMatrix();

    //belakang 
    glPushMatrix();
    glTranslatef(0, 500, 128);
    glColor3f(1.0f, 0.51f, 0.1f);
    glBegin(GL_POLYGON);
    glVertex3f(-400, 50, -400);
    glVertex3f(-500, 100, -480);
    glVertex3f(500, 100, -480);
    glVertex3f(400, 50, -400);
    glEnd(); 
    glPopMatrix();
}

void atap_kotak() {
    //depan 
    glPushMatrix();
    glTranslatef(0,510 ,184);
    glColor3f(1.0f, 0.35f, 0.0f);
    glBegin(GL_POLYGON);
    glVertex3f(-500, 30, -200);
    glVertex3f(-500, 0, -200);
    glVertex3f(500, 0, -200);
    glVertex3f(500, 30, -200);
    glEnd();
    glPopMatrix();

    //belakang
    glPushMatrix();
    glTranslatef(0, 512, -1474);
    glColor3f(1.0f, 0.35f, 0.0f);
    glBegin(GL_POLYGON);
    glVertex3f(-500, 30, 500);
    glVertex3f(-500, 0, 500);
    glVertex3f(500, 0, 500);
    glVertex3f(500, 30, 500);
    glEnd();
    glPopMatrix();

    //kiri
    glPushMatrix();
    glTranslatef(0, 510, -516);
    glColor3f(1.0f, 0.35f, 0.0f);
    glBegin(GL_POLYGON);
    glVertex3f(-500, 30, -460);
    glVertex3f(-500, 0, -460);
    glVertex3f(-500, 0, 500);
    glVertex3f(-500, 30, 500);
    glEnd();
    glPopMatrix();

    //KANAN
    glPushMatrix();
    glTranslatef(0, 513, -515);
    glColor3f(1.0f, 0.35f, 0.0f);
    glBegin(GL_POLYGON);
    glVertex3f(500, 30, -460);
    glVertex3f(500, 0, -460);
    glVertex3f(500, 0, 500);
    glVertex3f(500, 30, 500);
    glEnd();
    glPopMatrix();

    //atas
    glPushMatrix();
    glTranslatef(0, 512, -515);
    glColor3f(0.22, 0.22, 0.22);
    glBegin(GL_POLYGON);
    glVertex3f(-500, 30, 500);
    glVertex3f(-500, 30, -460);
    glVertex3f(500, 30, -460);
    glVertex3f(500, 30, 500);
    glEnd();
    glPopMatrix();
}


void lantai2() {
    //lantai2
    //depan 
    glPushMatrix();
    glTranslatef(0, 0, 224);
    glColor3f(0.95f, 0.95f, 0.95f);
    glBegin(GL_POLYGON);
    glVertex3f(-350, 500, 40);
    glVertex3f(-350, 300, 40);
    glVertex3f(350, 300, 40);
    glVertex3f(350, 500, 40);
    glEnd();
    glPopMatrix();

    //belakang
    glPushMatrix();
    glColor3f(0.95f, 0.95f, 0.95f);
    glBegin(GL_POLYGON);
    glVertex3f(-350, 500, -350);
    glVertex3f(-350, 300, -350);
    glVertex3f(350, 300, -270);
    glVertex3f(350, 500, -270);
    glEnd();
    glPopMatrix();

    //kiri
    glPushMatrix();
    glColor3f(0.95f, 0.95f, 0.95f);
    glBegin(GL_POLYGON);
    glVertex3f(-350, 500, -350);
    glVertex3f(-350, 300, -350);
    glVertex3f(-350, 300, 270);
    glVertex3f(-350, 500, 270);
    glEnd();
    glPopMatrix();

    //KANAN
    glPushMatrix();
    glColor3f(0.95f, 0.95f, 0.95f);
    glBegin(GL_POLYGON);
    glVertex3f(350, 500, -350);
    glVertex3f(350, 300, -350);
    glVertex3f(350, 300, 270);
    glVertex3f(350, 500, 270);
    glEnd();
    glPopMatrix();

    //atas
    glPushMatrix();
    glColor3f(0.95f, 0.95f, 0.95f);
    glBegin(GL_POLYGON);
    glVertex3f(-350, 500, -350);
    glVertex3f(350, 500, -350);
    glVertex3f(350, 500, 270);
    glVertex3f(-350, 500, 270);
    glEnd();
    glPopMatrix();
}

void atap1() {
    //atap
 //atas 
    glPushMatrix();
    glTranslatef(0, 350, 0);
    glColor3f(0.13f, 0.13f, 0.13f);
    glBegin(GL_POLYGON);
    glVertex3f(-450, 150, -500);
    glVertex3f(450, 150, -500);
    glVertex3f(450, 150, 500);
    glVertex3f(-450, 150, 500);
    glEnd();
    glPopMatrix();

    //kanan
    glPushMatrix();
    glTranslatef(0, 500, 0);
    glColor3f(0.13f, 0.13f, 0.13f);
    glBegin(GL_POLYGON);
    glVertex3f(450, 0, -500);
    glVertex3f(300, 100, -170);
    glVertex3f(300, 100, 170);
    glVertex3f(450, 0, 500); glEnd();
    glPopMatrix();

    //kiri 
    glPushMatrix();
    glTranslatef(0, 500, 0);
    glColor3f(0.13f, 0.13f, 0.13f);
    glBegin(GL_POLYGON);
    glVertex3f(-450, 0, -500);
    glVertex3f(-300, 100, -170);
    glVertex3f(-300, 100, 170);
    glVertex3f(-450, 0, 500);
    glEnd();
    glPopMatrix();

    //belakang 
    glPushMatrix();
    glTranslatef(0, 500, 0);
    glColor3f(0.13f, 0.13f, 0.13f);
    glBegin(GL_POLYGON);
    glVertex3f(-450, 0, -500);
    glVertex3f(-300, 100, -170);
    glVertex3f(300, 100, -170);
    glVertex3f(450, 0, -500);
    glEnd(); 
    glPopMatrix();

    //depan 
    glPushMatrix();
    glTranslatef(0, 500, 0);
    glColor3f(0.13f, 0.13f, 0.13f);
    glBegin(GL_POLYGON);
    glVertex3f(-450, 0, 500);
    glVertex3f(-300, 100, 170);
    glVertex3f(300, 100, 170);
    glVertex3f(450, 0, 500);
    glEnd();
    glPopMatrix();
}

void lantai3() {
    //lantai 3
    //dasar
    glPushMatrix();
    glTranslatef(0, 0, 224);
    glColor3f(0.95f, 0.95f, 0.95f);
    glBegin(GL_POLYGON);
    glVertex3f(-300, 480, 100);
    glVertex3f(-300, 300, 100);
    glVertex3f(300, 300, 100);
    glVertex3f(300, 480, 100);
    glEnd();
    glPopMatrix();

    //belakang
    glPushMatrix();
    glColor3f(0.95f, 0.95f, 0.95f);
    glBegin(GL_POLYGON);
    glVertex3f(-300, 480, -300);
    glVertex3f(-300, 300, -300);
    glVertex3f(300, 300, -300);
    glVertex3f(300, 480, -300);
    glEnd();
    glPopMatrix();

    //kiri
    glPushMatrix();
    glColor3f(0.95f, 0.95f, 0.95f);
    glBegin(GL_POLYGON);
    glVertex3f(-300, 480, -300);
    glVertex3f(-300, 300, -300);
    glVertex3f(-300, 300, 330);
    glVertex3f(-300, 480, 330);
    glEnd();
    glPopMatrix();

    //KANAN
    glPushMatrix();
    glColor3f(0.95f, 0.95f, 0.95f);
    glBegin(GL_POLYGON);
    glVertex3f(300, 480, -300);
    glVertex3f(300, 300, -300);
    glVertex3f(300, 300, 330);
    glVertex3f(300, 480, 330);
    glEnd();
    glPopMatrix();

    //atas
    glPushMatrix();
    glColor3f(0.95f, 0.95f, 0.95f);
    glBegin(GL_POLYGON);
    glVertex3f(-300, 480, -300);
    glVertex3f(300, 480, -300);
    glVertex3f(300, 480, 330);
    glVertex3f(-300, 480, 330);
    glEnd();
    glPopMatrix();
}

void atap2() {
  
       //atas 
    glPushMatrix();
    glTranslatef(0, 250.5, 0);
    glColor3f(0.13f, 0.13f, 0.13f);
    glBegin(GL_POLYGON);
    glVertex3f(-400, 300, -500);
    glVertex3f(400, 300, -500);
    glVertex3f(400, 300, 500);
    glVertex3f(-400, 300, 500);
    glEnd();
    glPopMatrix();

    //KANAN 
    glPushMatrix();
    glTranslatef(0, 500, 0);
    glColor3f(0.13f, 0.13f, 0.13f);
    glBegin(GL_POLYGON);
    glVertex3f(400, 50, -500);
    glVertex3f(200, 100, -170);
    glVertex3f(200, 100, 170);
    glVertex3f(400, 50, 500); glEnd();
    glPopMatrix();

    //kiri 
    glPushMatrix();
    glTranslatef(0, 500, 0);
    glColor3f(0.13f, 0.13f, 0.13f);
    glBegin(GL_POLYGON);
    glVertex3f(-400, 50, -500);
    glVertex3f(-200, 100, -170);
    glVertex3f(-200, 100, 170);
    glVertex3f(-400, 50, 500);
    glEnd();
    glPopMatrix();

    //belakang 
    glPushMatrix();
    glTranslatef(0, 500, 0);
    glColor3f(0.13f, 0.13f, 0.13f);
    glBegin(GL_POLYGON);
    glVertex3f(-400, 50, -500);
    glVertex3f(-200, 100, -170);
    glVertex3f(200, 100, -170);
    glVertex3f(400, 50, -500);
    glEnd(); glPopMatrix();

    //depan 
    glPushMatrix();
    glTranslatef(0, 500, 0);
    glColor3f(0.13f, 0.13f, 0.13f);
    glBegin(GL_POLYGON);
    glVertex3f(-400, 50, 500);
    glVertex3f(-200, 100, 170);
    glVertex3f(200, 100, 170);
    glVertex3f(400, 50, 500);
    glEnd();
    glPopMatrix();
}

void lantai4() {
    //lantai 4
//dasar
    glPushMatrix();
    glTranslatef(0, 500, 0);
    glColor3f(0.95f, 0.95f, 0.95f);
    glBegin(GL_POLYGON);
    glVertex3f(-200, 300, -100);
    glVertex3f(200, 300, -100);
    glVertex3f(200, 300, 300);
    glVertex3f(-200, 300, 300);
    glEnd();
    glPopMatrix();

    //kanan 
    glPushMatrix();
    glTranslatef(0, 501, 0);
    glColor3f(0.95f, 0.95f, 0.95f);
    glBegin(GL_POLYGON);
    glVertex3f(200, 450, -100);
    glVertex3f(200, 300, -100);
    glVertex3f(200, 300, 300);
    glVertex3f(200, 450, 300);
    glEnd();
    glPopMatrix();

    //kiri 
    glPushMatrix();
    glTranslatef(0, 501, 0);
    glColor3f(0.95f, 0.95f, 0.95f);
    glBegin(GL_POLYGON);
    glVertex3f(-200, 450, -100);
    glVertex3f(-200, 300, -100);
    glVertex3f(-200, 300, 300);
    glVertex3f(-200, 450, 300);
    glEnd();
    glPopMatrix();

    //depan
    glPushMatrix();
    glTranslatef(0, 501, 0);
    glColor3f(0.95f, 0.95f, 0.95f);
    glBegin(GL_POLYGON);
    glVertex3f(-200, 450, 300);
    glVertex3f(-200, 300, 300);
    glVertex3f(200, 300, 300);
    glVertex3f(200, 450, 300);
    glEnd();
    glPopMatrix();

    //belakang
    glPushMatrix();
    glTranslatef(-1.5, 502, 151);
    glColor3f(0.95f, 0.95f, 0.95f);
    glBegin(GL_POLYGON);
    glVertex3f(-200, 450, -250);
    glVertex3f(-200, 300, -250);
    glVertex3f(200, 300, -250);
    glVertex3f(200, 450, -250);
    glEnd();
    glPopMatrix();

    //atas
    glPushMatrix();
    glTranslatef(0, 502, 0);
    glColor3f(0.95f, 0.95f, 0.95f);
    glBegin(GL_POLYGON);
    glVertex3f(-200, 450, -100);
    glVertex3f(200, 450, -100);
    glVertex3f(200, 450, 300);
    glVertex3f(-200, 450, 300);
    glEnd();
    glPopMatrix();
}

void atap3() {

    //atap
    //atas 
    glPushMatrix();
    glTranslatef(0, 250.5, 0);
    glColor3f(0.13f, 0.13f, 0.13f);
    glBegin(GL_POLYGON);
    glVertex3f(-350, 300, -400);
    glVertex3f(350, 300, -400);
    glVertex3f(350, 300, 400);
    glVertex3f(-350, 300, 400);
    glEnd();
    glPopMatrix();

    //KANAN 
    glPushMatrix();
    glTranslatef(0, 500, 0);
    glColor3f(0.13f, 0.13f, 0.13f);
    glBegin(GL_POLYGON);
    glVertex3f(350, 50, -400);
    glVertex3f(175, 200, -170);
    glVertex3f(175, 200, 170);
    glVertex3f(350, 50, 400); glEnd();
    glPopMatrix();

    //kiri 
    glPushMatrix();
    glTranslatef(0, 500, 0);
    glColor3f(0.13f, 0.13f, 0.13f);
    glBegin(GL_POLYGON);
    glVertex3f(-350, 50, -400);
    glVertex3f(-175, 200, -170);
    glVertex3f(-175, 200, 170);
    glVertex3f(-350, 50, 400);
    glEnd();
    glPopMatrix();

    //belakang 
    glPushMatrix();
    glTranslatef(0, 500, 0);
    glColor3f(0.13f, 0.13f, 0.13f);
    glBegin(GL_POLYGON);
    glVertex3f(-350, 50, -400);
    glVertex3f(-175, 200, -170);
    glVertex3f(175, 200, -170);
    glVertex3f(350, 50, -400);
    glEnd();
    glPopMatrix();

    //depan 
    glPushMatrix();
    glTranslatef(0, 500, 0);
    glColor3f(0.13f, 0.13f, 0.13f);
    glBegin(GL_POLYGON);
    glVertex3f(-350, 50, 400);
    glVertex3f(-175, 200, 170);
    glVertex3f(175, 200, 170);
    glVertex3f(350, 50, 400);
    glEnd();
    glPopMatrix();

    //atas
    glPushMatrix();
    glTranslatef(0, 650, 0);
    glColor3f(0.13f, 0.13f, 0.13f);
    glBegin(GL_POLYGON);
    glVertex3f(-175, 50, 170);
    glVertex3f(-175, 50, -170);
    glVertex3f(175, 50, -170);
    glVertex3f(175, 50, 170);
    glEnd();
    glPopMatrix();
}





void pagardepan1() {

    glColor3f(1.0, 0.27, 0.0);
    glPushMatrix();
    glTranslatef(283, 297, 4);
    glScalef(3.5, 0.10, 0.2);
    glutSolidCube(100);
    glPopMatrix();


    glColor3f(1.0, 0.27, 0.0);
    glPushMatrix();
    glTranslatef(283, 245, 4);
    glScalef(3.5, 0.10, 0.2);
    glutSolidCube(100);
    glPopMatrix();


    glColor3f(1.0, 0.27, 0.0);
    glPushMatrix();
    glTranslatef(-283, 297, 4);
    glScalef(3.5, 0.10, 0.2);
    glutSolidCube(100);
    glPopMatrix();


    glColor3f(1.0, 0.27, 0.0);
    glPushMatrix();
    glTranslatef(-283, 245, 4);
    glScalef(3.5, 0.10, 0.2);
    glutSolidCube(100);
    glPopMatrix();


    glColor3f(1.0, 0.27, 0.0);
    glPushMatrix();
    glTranslatef(113, 248, -5);
    glScalef(0.2, 1.0, 0.1);
    glutSolidCube(100);
    glPopMatrix();

    glColor3f(1.0, 0.27, 0.0);
    glPushMatrix();
    glTranslatef(231, 248, -5);
    glScalef(0.2, 1.0, 0.1);
    glutSolidCube(100);
    glPopMatrix();

    glColor3f(1.0, 0.27, 0.0);
    glPushMatrix();
    glTranslatef(344, 248, -5);
    glScalef(0.2, 1.0, 0.1);
    glutSolidCube(100);
    glPopMatrix();

    glColor3f(1.0, 0.27, 0.0);
    glPushMatrix();
    glTranslatef(457, 248, -5);
    glScalef(0.2, 1.0, 0.1);
    glutSolidCube(100);
    glPopMatrix();


    glColor3f(1.0, 0.27, 0.0);
    glPushMatrix();
    glTranslatef(-457, 248, 5);
    glScalef(0.2, 1.0, 0.1);
    glutSolidCube(100);
    glPopMatrix();

    glColor3f(1.0, 0.27, 0.0);
    glPushMatrix();
    glTranslatef(-344, 248, 5);
    glScalef(0.2, 1.0, 0.1);
    glutSolidCube(100);
    glPopMatrix();

    glColor3f(1.0, 0.27, 0.0);
    glPushMatrix();
    glTranslatef(-231, 248, 5);
    glScalef(0.2, 1.0, 0.1);
    glutSolidCube(100);
    glPopMatrix();

    glColor3f(1.0, 0.27, 0.0);
    glPushMatrix();
    glTranslatef(-113, 248, 5);
    glScalef(0.2, 1.0, 0.1);
    glutSolidCube(100);
    glPopMatrix();
}

void pagarbelakang1() {
    
    glColor3f(1.0, 0.27, 0.0);
    glPushMatrix();
    glTranslatef(283, 297, -989);
    glScalef(3.5, 0.10, 0.2);
    glutSolidCube(100);
    glPopMatrix();

    glColor3f(1.0, 0.27, 0.0);
    glPushMatrix();
    glTranslatef(283, 245, -989);
    glScalef(3.5, 0.10, 0.2);
    glutSolidCube(100);
    glPopMatrix();


    glColor3f(1.0, 0.27, 0.0);
    glPushMatrix();
    glTranslatef(-283, 297, -989);
     glScalef(3.5, 0.10, 0.2);
    glutSolidCube(100);
    glPopMatrix();


    glColor3f(1.0, 0.27, 0.0);
    glPushMatrix();
    glTranslatef(-283, 245, -989);
    glScalef(3.5, 0.10, 0.2);
    glutSolidCube(100);
    glPopMatrix();


    glColor3f(1.0, 0.27, 0.0);
    glPushMatrix();
    glTranslatef(113, 248, -989);
    glScalef(0.2, 1.0, 0.1);
    glutSolidCube(100);
    glPopMatrix();

    glColor3f(1.0, 0.27, 0.0);
    glPushMatrix();
    glTranslatef(231, 248, -989);
    glScalef(0.2, 1.0, 0.1);
    glutSolidCube(100);
    glPopMatrix();

    glColor3f(1.0, 0.27, 0.0);
    glPushMatrix();
    glTranslatef(344, 248, -989);
    glScalef(0.2, 1.0, 0.1);
    glutSolidCube(100);
    glPopMatrix();

    glColor3f(1.0, 0.27, 0.0);
    glPushMatrix();
    glTranslatef(457, 248, -989);
    glScalef(0.2, 1.0, 0.1);
    glutSolidCube(100);
    glPopMatrix();


    glColor3f(1.0, 0.27, 0.0);
    glPushMatrix();
    glTranslatef(-113, 248, -989);
    glScalef(0.2, 1.0, 0.1);
    glutSolidCube(100);
    glPopMatrix();

    glColor3f(1.0, 0.27, 0.0);
    glPushMatrix();
    glTranslatef(-231, 248, -989);
    glScalef(0.2, 1.0, 0.1);
    glutSolidCube(100);
    glPopMatrix();

    glColor3f(1.0, 0.27, 0.0);
    glPushMatrix();
    glTranslatef(-344, 248, -989);
    glScalef(0.2, 1.0, 0.1);
    glutSolidCube(100);
    glPopMatrix();

    glColor3f(1.0, 0.27, 0.0);
    glPushMatrix();
   glTranslatef(-457, 248, -989);
    glScalef(0.2, 1.0, 0.1);
    glutSolidCube(100);
    glPopMatrix();
}

void pagarkiri1() {

    glColor3f(1.0, 0.27, 0.0);
    glPushMatrix();
    glTranslatef(-463, 248, -867);
    glScalef(0.2, 1.0, 0.1);
    glutSolidCube(100);
    glPopMatrix();

    glColor3f(1.0, 0.27, 0.0);
    glPushMatrix();
    glTranslatef(-463, 248, -747);
    glScalef(0.2, 1.0, 0.1);
    glutSolidCube(100);
    glPopMatrix();

    glColor3f(1.0, 0.27, 0.0);
    glPushMatrix();
    glTranslatef(-463, 248, -632);
    glScalef(0.2, 1.0, 0.1);
    glutSolidCube(100);
    glPopMatrix();

    glColor3f(1.0, 0.27, 0.0);
    glPushMatrix();
    glTranslatef(-463, 248, -496);
    glScalef(0.2, 1.0, 0.1);
    glutSolidCube(100);
    glPopMatrix();

    glColor3f(1.0, 0.27, 0.0);
    glPushMatrix();
    glTranslatef(-463, 248, -369);
    glScalef(0.2, 1.0, 0.1);
    glutSolidCube(100);
    glPopMatrix();

    glColor3f(1.0, 0.27, 0.0);
    glPushMatrix();
    glTranslatef(-463, 248, -242);
    glScalef(0.2, 1.0, 0.1);
    glutSolidCube(100);
    glPopMatrix();

    glColor3f(1.0, 0.27, 0.0);
    glPushMatrix();
    glTranslatef(-463, 248, -114);
    glScalef(0.2, 1.0, 0.1);
    glutSolidCube(100);
    glPopMatrix();



    glColor3f(1.0, 0.27, 0.0);
    glPushMatrix();
    glTranslatef(-452, 298, -486);
    glScalef(0.2, 0.10, 10);
    glutSolidCube(100);
    glPopMatrix();

    glColor3f(1.0, 0.27, 0.0);
    glPushMatrix();
    glTranslatef(-452 , 244, -486 );
    glScalef(0.2, 0.10, 10);
    glutSolidCube(100);
    glPopMatrix();
}

void pagarkanan1() {

    glColor3f(1.0, 0.27, 0.0);
    glPushMatrix();
    glTranslatef(463, 248, -867);
    glScalef(0.2, 1.0, 0.1);
    glutSolidCube(100);
    glPopMatrix();

    glColor3f(1.0, 0.27, 0.0);
    glPushMatrix();
    glTranslatef(463, 248, -747);
    glScalef(0.2, 1.0, 0.1);
    glutSolidCube(100);
    glPopMatrix();

    glColor3f(1.0, 0.27, 0.0);
    glPushMatrix();
    glTranslatef(463, 248, -632);
    glScalef(0.2, 1.0, 0.1);
    glutSolidCube(100);
    glPopMatrix();

    glColor3f(1.0, 0.27, 0.0);
    glPushMatrix();
    glTranslatef(463, 248, -496);
    glScalef(0.2, 1.0, 0.1);
    glutSolidCube(100);
    glPopMatrix();

    glColor3f(1.0, 0.27, 0.0);
    glPushMatrix();
    glTranslatef(463, 248, -369);
    glScalef(0.2, 1.0, 0.1);
    glutSolidCube(100);
    glPopMatrix();

    glColor3f(1.0, 0.27, 0.0);
    glPushMatrix();
    glTranslatef(463, 248, -242);
    glScalef(0.2, 1.0, 0.1);
    glutSolidCube(100);
    glPopMatrix();

    glColor3f(1.0, 0.27, 0.0);
    glPushMatrix();
    glTranslatef(463, 248, -114);
    glScalef(0.2, 1.0, 0.1);
    glutSolidCube(100);
    glPopMatrix();



    glColor3f(1.0, 0.27, 0.0);
    glPushMatrix();
    glTranslatef(452, 298, -486);
    glScalef(0.2, 0.10, 10);
    glutSolidCube(100);
    glPopMatrix();


    glColor3f(1.0, 0.27, 0.0);
    glPushMatrix();
    glTranslatef(452, 244, -486);
    glScalef(0.2, 0.10, 10);
    glutSolidCube(100);
    glPopMatrix();

}

//  LANTAI 2 PAGAR

void pagardepan2() {
  
    glColor3f(1.0, 0.27, 0.0);
    glPushMatrix();
    glTranslatef(283, 297, 0);
    glScalef(4.0, 0.08, 0.2);
    glutSolidCube(100);
    glPopMatrix();

    
    glColor3f(1.0, 0.27, 0.0);
    glPushMatrix();
    glTranslatef(283, 245, 0);
    glScalef(4.0, 0.08, 0.2);
    glutSolidCube(100);
    glPopMatrix();

  
    glColor3f(1.0, 0.27, 0.0);
    glPushMatrix();
    glTranslatef(-283, 297, 0);
    glScalef(4.0, 0.08, 0.2);
    glutSolidCube(100);
    glPopMatrix();


    glColor3f(1.0, 0.27, 0.0);
    glPushMatrix();
    glTranslatef(-283, 245, 0);
    glScalef(4.0, 0.08, 0.2);
    glutSolidCube(100);
    glPopMatrix();


    glColor3f(1.0, 0.27, 0.0);
    glPushMatrix();
    glTranslatef(88, 248, -5);
    glScalef(0.1, 1.0, 0.1);
    glutSolidCube(100);
    glPopMatrix();

    glColor3f(1.0, 0.27, 0.0);
    glPushMatrix();
    glTranslatef(166, 248, -5);
    glScalef(0.1, 1.0, 0.1);
    glutSolidCube(100);
    glPopMatrix();

    glColor3f(1.0, 0.27, 0.0);
    glPushMatrix();
    glTranslatef(249, 248, -5);
    glScalef(0.1, 1.0, 0.1);
    glutSolidCube(100);
    glPopMatrix();

    glColor3f(1.0, 0.27, 0.0);
    glPushMatrix();
    glTranslatef(330 , 248, -5);
    glScalef(0.1, 1.0, 0.1);
    glutSolidCube(100);
    glPopMatrix();

    glColor3f(1.0, 0.27, 0.0);
    glPushMatrix();
    glTranslatef(412, 248, -5);
    glScalef(0.1, 1.0, 0.1);
    glutSolidCube(100);
    glPopMatrix();

    glColor3f(1.0, 0.27, 0.0);
    glPushMatrix();
    glTranslatef(477, 248, -5);
    glScalef(0.1, 1.0, 0.1);
    glutSolidCube(100);
    glPopMatrix();


    glColor3f(1.0, 0.27, 0.0);
    glPushMatrix();
    glTranslatef(-88, 248, -5);
    glScalef(0.1, 1.0, 0.1);
    glutSolidCube(100);
    glPopMatrix();

    glColor3f(1.0, 0.27, 0.0);
    glPushMatrix();
    glTranslatef(-166, 248, -5);
    glScalef(0.1, 1.0, 0.1);
    glutSolidCube(100);
    glPopMatrix();

    glColor3f(1.0, 0.27, 0.0);
    glPushMatrix();
    glTranslatef(-249, 248, -5);
    glScalef(0.1, 1.0, 0.1);
    glutSolidCube(100);
    glPopMatrix();

    glColor3f(1.0, 0.27, 0.0);
    glPushMatrix();
    glTranslatef(-330, 248, -5);
    glScalef(0.1, 1.0, 0.1);
    glutSolidCube(100);
    glPopMatrix();

    glColor3f(1.0, 0.27, 0.0);
    glPushMatrix();
    glTranslatef(-412, 248, -5);
    glScalef(0.1, 1.0, 0.1);
    glutSolidCube(100);
    glPopMatrix();

    glColor3f(1.0, 0.27, 0.0);
    glPushMatrix();
    glTranslatef(-477, 248, -5);
    glScalef(0.1, 1.0, 0.1);
    glutSolidCube(100);
    glPopMatrix();
  
}

void pagarkiri2() {


    glColor3f(1.0, 0.27, 0.0);
    glPushMatrix();
    glTranslatef(-488, 245, -465);
    glScalef(0.1, 0.07, 9.5);
    glutSolidCube(100);
    glPopMatrix();

    glColor3f(1.0, 0.27, 0.0);
    glPushMatrix();
    glTranslatef(-488, 298, -465);
    glScalef(0.1, 0.07, 9.5);
    glutSolidCube(100);
    glPopMatrix();


    glColor3f(1.0, 0.27, 0.0);
    glPushMatrix();
    glTranslatef(-488, 248, -935);
    glScalef(0.1, 1.0, 0.1);
    glutSolidCube(100);
    glPopMatrix();

    glColor3f(1.0, 0.27, 0.0);
    glPushMatrix();
    glTranslatef(-488, 248, -864);
    glScalef(0.1, 1.0, 0.1);
    glutSolidCube(100);
    glPopMatrix();

    glColor3f(1.0, 0.27, 0.0);
    glPushMatrix();
    glTranslatef(-488, 248, -793);
    glScalef(0.1, 1.0, 0.1);
    glutSolidCube(100);
    glPopMatrix();

    glColor3f(1.0, 0.27, 0.0);
    glPushMatrix();
    glTranslatef(-488, 248, -722);
    glScalef(0.1, 1.0, 0.1);
    glutSolidCube(100);
    glPopMatrix();

    glColor3f(1.0, 0.27, 0.0);
    glPushMatrix();
    glTranslatef(-488, 248, -651);
    glScalef(0.1, 1.0, 0.1);
    glutSolidCube(100);
    glPopMatrix();

    glColor3f(1.0, 0.27, 0.0);
    glPushMatrix();
    glTranslatef(-488, 248, -580);
    glScalef(0.1, 1.0, 0.1);
    glutSolidCube(100);
    glPopMatrix();

    glColor3f(1.0, 0.27, 0.0);
    glPushMatrix();
    glTranslatef(-488, 248, -509);
    glScalef(0.1, 1.0, 0.1);
    glutSolidCube(100);
    glPopMatrix();

    glColor3f(1.0, 0.27, 0.0);
    glPushMatrix();
    glTranslatef(-488, 248, -438);
    glScalef(0.1, 1.0, 0.1);
    glutSolidCube(100);
    glPopMatrix();

    glColor3f(1.0, 0.27, 0.0);
    glPushMatrix();
    glTranslatef(-488, 248, -367);
    glScalef(0.1, 1.0, 0.1);
    glutSolidCube(100);
    glPopMatrix();

    glColor3f(1.0, 0.27, 0.0);
    glPushMatrix();
    glTranslatef(-488, 248, -296);
    glScalef(0.1, 1.0, 0.1);
    glutSolidCube(100);
    glPopMatrix();

    glColor3f(1.0, 0.27, 0.0);
    glPushMatrix();
    glTranslatef(-488, 248, -225);
    glScalef(0.1, 1.0, 0.1);
    glutSolidCube(100);
    glPopMatrix();

    glColor3f(1.0, 0.27, 0.0);
    glPushMatrix();
    glTranslatef(-488, 248, -154);
    glScalef(0.1, 1.0, 0.1);
    glutSolidCube(100);
    glPopMatrix();

    glColor3f(1.0, 0.27, 0.0);
    glPushMatrix();
    glTranslatef(-488, 248, -83);
    glScalef(0.1, 1.0, 0.1);
    glutSolidCube(100);
    glPopMatrix();

    glColor3f(1.0, 0.27, 0.0);
    glPushMatrix();
    glTranslatef(-488, 248, -12);
    glScalef(0.1, 1.0, 0.1);
    glutSolidCube(100);
    glPopMatrix();
    
}

void pagarkanan2() {


    glColor3f(1.0, 0.27, 0.0);
    glPushMatrix();
    glTranslatef(488, 245, -465);
    glScalef(0.1, 0.07, 9.5);
    glutSolidCube(100);
    glPopMatrix();


    glColor3f(1.0, 0.27, 0.0);
    glPushMatrix();
    glTranslatef(488, 298, -465);
    glScalef(0.1, 0.07, 9.5);
    glutSolidCube(100);
    glPopMatrix();


    glColor3f(1.0, 0.27, 0.0);
    glPushMatrix();
    glTranslatef(488, 248, -935);
    glScalef(0.1, 1.0, 0.1);
    glutSolidCube(100);
    glPopMatrix();

    glColor3f(1.0, 0.27, 0.0);
    glPushMatrix();
    glTranslatef(488, 248, -864);
    glScalef(0.1, 1.0, 0.1);
    glutSolidCube(100);
    glPopMatrix();

    glColor3f(1.0, 0.27, 0.0);
    glPushMatrix();
    glTranslatef(488, 248, -793);
    glScalef(0.1, 1.0, 0.1);
    glutSolidCube(100);
    glPopMatrix();

    glColor3f(1.0, 0.27, 0.0);
    glPushMatrix();
    glTranslatef(488, 248, -722);
    glScalef(0.1, 1.0, 0.1);
    glutSolidCube(100);
    glPopMatrix();

    glColor3f(1.0, 0.27, 0.0);
    glPushMatrix();
    glTranslatef(488, 248, -651);
    glScalef(0.1, 1.0, 0.1);
    glutSolidCube(100);
    glPopMatrix();

    glColor3f(1.0, 0.27, 0.0);
    glPushMatrix();
    glTranslatef(488, 248, -580);
    glScalef(0.1, 1.0, 0.1);
    glutSolidCube(100);
    glPopMatrix();

    glColor3f(1.0, 0.27, 0.0);
    glPushMatrix();
    glTranslatef(488, 248, -509);
    glScalef(0.1, 1.0, 0.1);
    glutSolidCube(100);
    glPopMatrix();

    glColor3f(1.0, 0.27, 0.0);
    glPushMatrix();
    glTranslatef(488, 248, -438);
    glScalef(0.1, 1.0, 0.1);
    glutSolidCube(100);
    glPopMatrix();

    glColor3f(1.0, 0.27, 0.0);
    glPushMatrix();
    glTranslatef(488, 248, -367);
    glScalef(0.1, 1.0, 0.1);
    glutSolidCube(100);
    glPopMatrix();

    glColor3f(1.0, 0.27, 0.0);
    glPushMatrix();
    glTranslatef(488, 248, -296);
    glScalef(0.1, 1.0, 0.1);
    glutSolidCube(100);
    glPopMatrix();

    glColor3f(1.0, 0.27, 0.0);
    glPushMatrix();
    glTranslatef(488, 248, -225);
    glScalef(0.1, 1.0, 0.1);
    glutSolidCube(100);
    glPopMatrix();

    glColor3f(1.0, 0.27, 0.0);
    glPushMatrix();
    glTranslatef(488, 248, -154);
    glScalef(0.1, 1.0, 0.1);
    glutSolidCube(100);
    glPopMatrix();

    glColor3f(1.0, 0.27, 0.0);
    glPushMatrix();
    glTranslatef(488, 248, -83);
    glScalef(0.1, 1.0, 0.1);
    glutSolidCube(100);
    glPopMatrix();

    glColor3f(1.0, 0.27, 0.0);
    glPushMatrix();
    glTranslatef(488, 248, -12);
    glScalef(0.1, 1.0, 0.1);
    glutSolidCube(100);
    glPopMatrix();
}
    void pagarbelakang2(){
 
        glColor3f(1.0, 0.27, 0.0);
    glPushMatrix();
    glTranslatef(283, 297, -989);
    glScalef(4.0, 0.08, 0.2);
    glutSolidCube(100);
    glPopMatrix();


    glColor3f(1.0, 0.27, 0.0);
    glPushMatrix();
    glTranslatef(283, 245, -989);
    glScalef(4.0, 0.08, 0.2);
    glutSolidCube(100);
    glPopMatrix();


    glColor3f(1.0, 0.27, 0.0);
    glPushMatrix();
    glTranslatef(-283, 297, -989);
    glScalef(4.0, 0.08, 0.2);
    glutSolidCube(100);
    glPopMatrix();


    glColor3f(1.0, 0.27, 0.0);
    glPushMatrix();
    glTranslatef(-283, 245, -989);
    glScalef(4.0, 0.08, 0.2);
    glutSolidCube(100);
    glPopMatrix();



    glColor3f(1.0, 0.27, 0.0);
    glPushMatrix();
    glTranslatef(88, 248, -989);
    glScalef(0.1, 1.0, 0.1);
    glutSolidCube(100);
    glPopMatrix();

    glColor3f(1.0, 0.27, 0.0);
    glPushMatrix();
    glTranslatef(166, 248, -989);
    glScalef(0.1, 1.0, 0.1);
    glutSolidCube(100);
    glPopMatrix();

    glColor3f(1.0, 0.27, 0.0);
    glPushMatrix();
    glTranslatef(249, 248, -989);
    glScalef(0.1, 1.0, 0.1);
    glutSolidCube(100);
    glPopMatrix();

    glColor3f(1.0, 0.27, 0.0);
    glPushMatrix();
    glTranslatef(330, 248, -989);
    glScalef(0.1, 1.0, 0.1);
    glutSolidCube(100);
    glPopMatrix();

    glColor3f(1.0, 0.27, 0.0);
    glPushMatrix();
    glTranslatef(412, 248, -989);
    glScalef(0.1, 1.0, 0.1);
    glutSolidCube(100);
    glPopMatrix();

    glColor3f(1.0, 0.27, 0.0);
    glPushMatrix();
    glTranslatef(477, 248, -989);
    glScalef(0.1, 1.0, 0.1);
    glutSolidCube(100);
    glPopMatrix();


    glColor3f(1.0, 0.27, 0.0);
    glPushMatrix();
    glTranslatef(-88, 248, -989);
    glScalef(0.1, 1.0, 0.1);
    glutSolidCube(100);
    glPopMatrix();

    glColor3f(1.0, 0.27, 0.0);
    glPushMatrix();
    glTranslatef(-166, 248, -989);
    glScalef(0.1, 1.0, 0.1);
    glutSolidCube(100);
    glPopMatrix();

    glColor3f(1.0, 0.27, 0.0);
    glPushMatrix();
    glTranslatef(-249, 248, -989);
    glScalef(0.1, 1.0, 0.1);
    glutSolidCube(100);
    glPopMatrix();

    glColor3f(1.0, 0.27, 0.0);
    glPushMatrix();
    glTranslatef(-330, 248, -989);
    glScalef(0.1, 1.0, 0.1);
    glutSolidCube(100);
    glPopMatrix();

    glColor3f(1.0, 0.27, 0.0);
    glPushMatrix();
    glTranslatef(-412, 248, -989);
    glScalef(0.1, 1.0, 0.1);
    glutSolidCube(100);
    glPopMatrix();

    glColor3f(1.0, 0.27, 0.0);
    glPushMatrix();
    glTranslatef(-477, 248, -989);
    glScalef(0.1, 1.0, 0.1);
    glutSolidCube(100);
    glPopMatrix();

}


    void dasar1() {
        //layer lantai merah
        glColor3f(0.22, 0.22, 0.22);
        glPushMatrix();
        glTranslatef(0, 952, -542);
        glScalef(1.45, 0.02, 2.00);
        glutSolidCube(500);
        glPopMatrix();

        glColor3f(1.0, 1.0, 1.0);
        glPushMatrix();
        glTranslatef(0, 945, -542);
        glScalef(1.60, 0.01, 2.00);
        glutSolidCube(500);
        glPopMatrix();
    }

    //LANTAI 3

    void pagardepan3() {
     
        glColor3f(1.0, 0.27, 0.0);
        glPushMatrix();
        glTranslatef(201, 297, -5);
        glScalef(3.0, 0.08, 0.2);
        glutSolidCube(100);
        glPopMatrix();


        glColor3f(1.0, 0.27, 0.0);
        glPushMatrix();
        glTranslatef(201, 259, -5);
        glScalef(3.0, 0.08, 0.2);
        glutSolidCube(100);
        glPopMatrix();


        glColor3f(1.0, 0.27, 0.0);
        glPushMatrix();
        glTranslatef(-201, 297, -5);
        glScalef(3.0, 0.08, 0.2);
        glutSolidCube(100);
        glPopMatrix();


        glColor3f(1.0, 0.27, 0.0);
        glPushMatrix();
        glTranslatef(-201, 259, -5);
        glScalef(3.0, 0.08, 0.2);
        glutSolidCube(100);
        glPopMatrix();


        glColor3f(1.0, 0.27, 0.0);
        glPushMatrix();
        glTranslatef(51, 261, -5);
        glScalef(0.08, 0.8, 0.1);
        glutSolidCube(100);
        glPopMatrix();

        glColor3f(1.0, 0.27, 0.0);
        glPushMatrix();
        glTranslatef(110, 261, -5);
        glScalef(0.08, 0.8, 0.1);
        glutSolidCube(100);
        glPopMatrix();

        glColor3f(1.0, 0.27, 0.0);
        glPushMatrix();
        glTranslatef(169, 261, -5);
        glScalef(0.08, 0.8, 0.1);
        glutSolidCube(100);
        glPopMatrix();

        glColor3f(1.0, 0.27, 0.0);
        glPushMatrix();
        glTranslatef(228, 261, -5);
        glScalef(0.08, 0.8, 0.1);
        glutSolidCube(100);
        glPopMatrix();

        glColor3f(1.0, 0.27, 0.0);
        glPushMatrix();
        glTranslatef(287, 261, -5);
        glScalef(0.08, 0.8, 0.1);
        glutSolidCube(100);
        glPopMatrix();

        glColor3f(1.0, 0.27, 0.0);
        glPushMatrix();
        glTranslatef(346, 261, -5);
        glScalef(0.08, 0.8, 0.1);
        glutSolidCube(100);
        glPopMatrix();

 
        glColor3f(1.0, 0.27, 0.0);
        glPushMatrix();
        glTranslatef(-51, 261, -5);
        glScalef(0.08, 0.8, 0.1);
        glutSolidCube(100);
        glPopMatrix();

        glColor3f(1.0, 0.27, 0.0);
        glPushMatrix();
        glTranslatef(-110, 261, -5);
        glScalef(0.08, 0.8, 0.1);
        glutSolidCube(100);
        glPopMatrix();

        glColor3f(1.0, 0.27, 0.0);
        glPushMatrix();
        glTranslatef(-169, 261, -5);
        glScalef(0.08, 0.8, 0.1);
        glutSolidCube(100);
        glPopMatrix();

        glColor3f(1.0, 0.27, 0.0);
        glPushMatrix();
        glTranslatef(-228, 261, -5);
        glScalef(0.08, 0.8, 0.1);
        glutSolidCube(100);
        glPopMatrix();

        glColor3f(1.0, 0.27, 0.0);
        glPushMatrix();
        glTranslatef(-287, 261, -5);
        glScalef(0.08, 0.8, 0.1);
        glutSolidCube(100);
        glPopMatrix();

        glColor3f(1.0, 0.27, 0.0);
        glPushMatrix();
        glTranslatef(-346, 261, -5);
        glScalef(0.08, 0.8, 0.1);
        glutSolidCube(100);
        glPopMatrix();
    }

    void pagarkiri3() {


        glColor3f(1.0, 0.27, 0.0);
        glPushMatrix();
        glTranslatef(-486, 243, -515);
        glScalef(0.08, 0.05, 9.5);
        glutSolidCube(100);
        glPopMatrix();

        glColor3f(1.0, 0.27, 0.0);
        glPushMatrix();
        glTranslatef(-486, 284, -515);
        glScalef(0.08, 0.05, 9.5);
        glutSolidCube(100);
        glPopMatrix();

        glColor3f(1.0, 0.27, 0.0);
        glPushMatrix();
        glTranslatef(-488, 248, -935);
        glScalef(0.08, 0.8, 0.1);
        glutSolidCube(100);
        glPopMatrix();

        glColor3f(1.0, 0.27, 0.0);
        glPushMatrix();
        glTranslatef(-488, 248, -864);
        glScalef(0.08, 0.8, 0.1);
        glutSolidCube(100);
        glPopMatrix();

        glColor3f(1.0, 0.27, 0.0);
        glPushMatrix();
        glTranslatef(-488, 248, -793);
        glScalef(0.08, 0.8, 0.1);
        glutSolidCube(100);
        glPopMatrix();

        glColor3f(1.0, 0.27, 0.0);
        glPushMatrix();
        glTranslatef(-488, 248, -722);
        glScalef(0.08, 0.8, 0.1);
        glutSolidCube(100);
        glPopMatrix();

        glColor3f(1.0, 0.27, 0.0);
        glPushMatrix();
        glTranslatef(-488, 248, -651);
        glScalef(0.08, 0.8, 0.1);
        glutSolidCube(100);
        glPopMatrix();

        glColor3f(1.0, 0.27, 0.0);
        glPushMatrix();
        glTranslatef(-488, 248, -580);
        glScalef(0.08, 0.8, 0.1);
        glutSolidCube(100);
        glPopMatrix();

        glColor3f(1.0, 0.27, 0.0);
        glPushMatrix();
        glTranslatef(-488, 248, -509);
        glScalef(0.08, 0.8, 0.1);
        glutSolidCube(100);
        glPopMatrix();

        glColor3f(1.0, 0.27, 0.0);
        glPushMatrix();
        glTranslatef(-488, 248, -438);
        glScalef(0.08, 0.8, 0.1);
        glutSolidCube(100);
        glPopMatrix();

        glColor3f(1.0, 0.27, 0.0);
        glPushMatrix();
        glTranslatef(-488, 248, -367);
        glScalef(0.08, 0.8, 0.1);
        glutSolidCube(100);
        glPopMatrix();

        glColor3f(1.0, 0.27, 0.0);
        glPushMatrix();
        glTranslatef(-488, 248, -296);
        glScalef(0.08, 0.8, 0.1);
        glutSolidCube(100);
        glPopMatrix();

        glColor3f(1.0, 0.27, 0.0);
        glPushMatrix();
        glTranslatef(-488, 248, -225);
        glScalef(0.08, 0.8, 0.1);
        glutSolidCube(100);
        glPopMatrix();

        glColor3f(1.0, 0.27, 0.0);
        glPushMatrix();
        glTranslatef(-488, 248, -154);
        glScalef(0.08, 0.8, 0.1);
        glutSolidCube(100);
        glPopMatrix();

        glColor3f(1.0, 0.27, 0.0);
        glPushMatrix();
        glTranslatef(-488, 248, -83);
        glScalef(0.08, 0.8, 0.1);
        glutSolidCube(100);
        glPopMatrix();
       
    }

    void pagarkanan3() {


        glColor3f(1.0, 0.27, 0.0);
        glPushMatrix();
        glTranslatef(486, 243, -515);
        glScalef(0.08, 0.05, 9.5);
        glutSolidCube(100);
        glPopMatrix();


        glColor3f(1.0, 0.27, 0.0);
        glPushMatrix();
        glTranslatef(486, 284, -515);
        glScalef(0.08, 0.05, 9.5);
        glutSolidCube(100);
        glPopMatrix();


        glColor3f(1.0, 0.27, 0.0);
        glPushMatrix();
        glTranslatef(488, 248, -935);
        glScalef(0.08, 0.8, 0.1);
        glutSolidCube(100);
        glPopMatrix();

        glColor3f(1.0, 0.27, 0.0);
        glPushMatrix();
        glTranslatef(488, 248, -864);
        glScalef(0.08, 0.8, 0.1);
        glutSolidCube(100);
        glPopMatrix();

        glColor3f(1.0, 0.27, 0.0);
        glPushMatrix();
        glTranslatef(488, 248, -793);
        glScalef(0.08, 0.8, 0.1);
        glutSolidCube(100);
        glPopMatrix();

        glColor3f(1.0, 0.27, 0.0);
        glPushMatrix();
        glTranslatef(488, 248, -722);
        glScalef(0.08, 0.8, 0.1);
        glutSolidCube(100);
        glPopMatrix();

        glColor3f(1.0, 0.27, 0.0);
        glPushMatrix();
        glTranslatef(488, 248, -651);
        glScalef(0.08, 0.8, 0.1);
        glutSolidCube(100);
        glPopMatrix();

        glColor3f(1.0, 0.27, 0.0);
        glPushMatrix();
        glTranslatef(488, 248, -580);
        glScalef(0.08, 0.8, 0.1);
        glutSolidCube(100);
        glPopMatrix();

        glColor3f(1.0, 0.27, 0.0);
        glPushMatrix();
        glTranslatef(488, 248, -509);
        glScalef(0.08, 0.8, 0.1);
        glutSolidCube(100);
        glPopMatrix();

        glColor3f(1.0, 0.27, 0.0);
        glPushMatrix();
        glTranslatef(488, 248, -438);
        glScalef(0.08, 0.8, 0.1);
        glutSolidCube(100);
        glPopMatrix();

        glColor3f(1.0, 0.27, 0.0);
        glPushMatrix();
        glTranslatef(488, 248, -367);
        glScalef(0.08, 0.8, 0.1);
        glutSolidCube(100);
        glPopMatrix();

        glColor3f(1.0, 0.27, 0.0);
        glPushMatrix();
        glTranslatef(488, 248, -296);
        glScalef(0.08, 0.8, 0.1);
        glutSolidCube(100);
        glPopMatrix();

        glColor3f(1.0, 0.27, 0.0);
        glPushMatrix();
        glTranslatef(488, 248, -225);
        glScalef(0.08, 0.8, 0.1);
        glutSolidCube(100);
        glPopMatrix();

        glColor3f(1.0, 0.27, 0.0);
        glPushMatrix();
        glTranslatef(488, 248, -154);
        glScalef(0.08, 0.8, 0.1);
        glutSolidCube(100);
        glPopMatrix();

        glColor3f(1.0, 0.27, 0.0);
        glPushMatrix();
        glTranslatef(488, 248, -83);
        glScalef(0.08, 0.8, 0.1);
        glutSolidCube(100);
        glPopMatrix();

    }

    void pagarbelakang3() {
 
        glColor3f(1.0, 0.27, 0.0);
        glPushMatrix();
        glTranslatef(201, 297, -980);
        glScalef(3.0, 0.08, 0.2);
        glutSolidCube(100);
        glPopMatrix();


        glColor3f(1.0, 0.27, 0.0);
        glPushMatrix();
        glTranslatef(201, 259, -980);
        glScalef(3.0, 0.08, 0.2);
        glutSolidCube(100);
        glPopMatrix();

        glColor3f(1.0, 0.27, 0.0);
        glPushMatrix();
        glTranslatef(-201, 297, -980);
        glScalef(3.0, 0.08, 0.2);
        glutSolidCube(100);
        glPopMatrix();


        glColor3f(1.0, 0.27, 0.0);
        glPushMatrix();
        glTranslatef(-201, 259, -980);
        glScalef(3.0, 0.08, 0.2);
        glutSolidCube(100);
        glPopMatrix();


        glColor3f(1.0, 0.27, 0.0);
        glPushMatrix();
        glTranslatef(51, 261, -980);
        glScalef(0.08, 0.8, 0.1);
        glutSolidCube(100);
        glPopMatrix();

        glColor3f(1.0, 0.27, 0.0);
        glPushMatrix();
        glTranslatef(110, 261, -980);
        glScalef(0.08, 0.8, 0.1);
        glutSolidCube(100);
        glPopMatrix();

        glColor3f(1.0, 0.27, 0.0);
        glPushMatrix();
        glTranslatef(169, 261, -980);
        glScalef(0.08, 0.8, 0.1);
        glutSolidCube(100);
        glPopMatrix();

        glColor3f(1.0, 0.27, 0.0);
        glPushMatrix();
        glTranslatef(228, 261, -980);
        glScalef(0.08, 0.8, 0.1);
        glutSolidCube(100);
        glPopMatrix();

        glColor3f(1.0, 0.27, 0.0);
        glPushMatrix();
        glTranslatef(287, 261, -980);
        glScalef(0.08, 0.8, 0.1);
        glutSolidCube(100);
        glPopMatrix();

        glColor3f(1.0, 0.27, 0.0);
        glPushMatrix();
        glTranslatef(346, 261, -980);
        glScalef(0.08, 0.8, 0.1);
        glutSolidCube(100);
        glPopMatrix();


        glColor3f(1.0, 0.27, 0.0);
        glPushMatrix();
        glTranslatef(-51, 261, -980);
        glScalef(0.08, 0.8, 0.1);
        glutSolidCube(100);
        glPopMatrix();

        glColor3f(1.0, 0.27, 0.0);
        glPushMatrix();
        glTranslatef(-110, 261, -980);
        glScalef(0.08, 0.8, 0.1);
        glutSolidCube(100);
        glPopMatrix();

        glColor3f(1.0, 0.27, 0.0);
        glPushMatrix();
        glTranslatef(-169, 261, -980);
        glScalef(0.08, 0.8, 0.1);
        glutSolidCube(100);
        glPopMatrix();

        glColor3f(1.0, 0.27, 0.0);
        glPushMatrix();
        glTranslatef(-228, 261, -980);
        glScalef(0.08, 0.8, 0.1);
        glutSolidCube(100);
        glPopMatrix();

        glColor3f(1.0, 0.27, 0.0);
        glPushMatrix();
        glTranslatef(-287, 261, -980);
        glScalef(0.08, 0.8, 0.1);
        glutSolidCube(100);
        glPopMatrix();

        glColor3f(1.0, 0.27, 0.0);
        glPushMatrix();
        glTranslatef(-346, 261, -980);
        glScalef(0.08, 0.8, 0.1);
        glutSolidCube(100);
        glPopMatrix();
    }

    void dasar2() {
        //layer lantai merah
        glColor3f(0.22, 0.22, 0.22);
        glPushMatrix();
        glTranslatef(0, 952, -542);
        glScalef(1.05, 0.01, 1.10);
        glutSolidCube(500);
        glPopMatrix();

        glColor3f(1.0, 1.0, 1.0);
        glPushMatrix();
        glTranslatef(0, 945, -542);
        glScalef(1.20, 0.01, 1.20);
        glutSolidCube(500);
        glPopMatrix();
    }

    //LANTAI 4

    void pagardepan4() {
 
        glColor3f(1.0, 0.27, 0.0);
        glPushMatrix();
        glTranslatef(6, 297, -80);
        glScalef(4.5, 0.05, 0.09);
        glutSolidCube(100);
        glPopMatrix();


        glColor3f(1.0, 0.27, 0.0);
        glPushMatrix();
        glTranslatef(6, 267, -80);
        glScalef(4.5, 0.05, 0.09);
        glutSolidCube(100);
        glPopMatrix();


        glColor3f(1.0, 0.27, 0.0);
        glPushMatrix();
        glTranslatef(229, 274, -79);
        glScalef(0.04, 0.5, 0.1);
        glutSolidCube(100);
        glPopMatrix();

        glColor3f(1.0, 0.27, 0.0);
        glPushMatrix();
        glTranslatef(188, 274, -79);
        glScalef(0.04, 0.5, 0.1);
        glutSolidCube(100);
        glPopMatrix();

        glColor3f(1.0, 0.27, 0.0);
        glPushMatrix();
        glTranslatef(147, 274, -79);
        glScalef(0.04, 0.5, 0.1);
        glutSolidCube(100);
        glPopMatrix();

        glColor3f(1.0, 0.27, 0.0);
        glPushMatrix();
        glTranslatef(106, 274, -79);
        glScalef(0.04, 0.5, 0.1);
        glutSolidCube(100);
        glPopMatrix();

        glColor3f(1.0, 0.27, 0.0);
        glPushMatrix();
        glTranslatef(65, 274, -79);
        glScalef(0.04, 0.5, 0.1);
        glutSolidCube(100);
        glPopMatrix();

        glColor3f(1.0, 0.27, 0.0);
        glPushMatrix();
        glTranslatef(24, 274, -79);
        glScalef(0.04, 0.5, 0.1);
        glutSolidCube(100);
        glPopMatrix();
     
        glColor3f(1.0, 0.27, 0.0);
        glPushMatrix();
        glTranslatef(-17, 274, -79);
        glScalef(0.04, 0.5, 0.1);
        glutSolidCube(100);
        glPopMatrix();

        glColor3f(1.0, 0.27, 0.0);
        glPushMatrix();
        glTranslatef(-58, 274, -79);
        glScalef(0.04, 0.5, 0.1);
        glutSolidCube(100);
        glPopMatrix();

        glColor3f(1.0, 0.27, 0.0);
        glPushMatrix();
        glTranslatef(-99, 274, -79);
        glScalef(0.04, 0.5, 0.1);
        glutSolidCube(100);
        glPopMatrix();

        glColor3f(1.0, 0.27, 0.0);
        glPushMatrix();
        glTranslatef(-140, 274, -79);
        glScalef(0.04, 0.5, 0.1);
        glutSolidCube(100);
        glPopMatrix();

        glColor3f(1.0, 0.27, 0.0);
        glPushMatrix();
        glTranslatef(-181, 274, -79);
        glScalef(0.04, 0.5, 0.1);
        glutSolidCube(100);
        glPopMatrix();

        glColor3f(1.0, 0.27, 0.0);
        glPushMatrix();
        glTranslatef(-222, 274, -79);
        glScalef(0.04, 0.5, 0.1);
        glutSolidCube(100);
        glPopMatrix();
       
    }

    void pagarkiri4() {


        glColor3f(1.0, 0.27, 0.0);
        glPushMatrix();
        glTranslatef(-486, 243, -515);
        glScalef(0.06, 0.05, 5.0);
        glutSolidCube(100);
        glPopMatrix();

        glColor3f(1.0, 0.27, 0.0);
        glPushMatrix();
        glTranslatef(-486, 275, -515);
        glScalef(0.06, 0.05, 5.0);
        glutSolidCube(100);
        glPopMatrix();


        glColor3f(1.0, 0.27, 0.0);
        glPushMatrix();
        glTranslatef(-490, 238, -762);
        glScalef(0.04, 0.8, 0.1);
        glutSolidCube(100);
        glPopMatrix();

        glColor3f(1.0, 0.27, 0.0);
        glPushMatrix();
        glTranslatef(-490 , 238 , -712 );
        glScalef(0.04, 0.8, 0.1);
        glutSolidCube(100);
        glPopMatrix();

        glColor3f(1.0, 0.27, 0.0);
        glPushMatrix();
        glTranslatef(-490, 238, -662);
        glScalef(0.04, 0.8, 0.1);
        glutSolidCube(100);
        glPopMatrix();

        glColor3f(1.0, 0.27, 0.0);
        glPushMatrix();
        glTranslatef(-490, 238, -612);
        glScalef(0.04, 0.8, 0.1);
        glutSolidCube(100);
        glPopMatrix();

        glColor3f(1.0, 0.27, 0.0);
        glPushMatrix();
        glTranslatef(-490, 238, -562);
        glScalef(0.04, 0.8, 0.1);
        glutSolidCube(100);
        glPopMatrix();

        glColor3f(1.0, 0.27, 0.0);
        glPushMatrix();
        glTranslatef(-490, 238, -512);
        glScalef(0.04, 0.8, 0.1);
        glutSolidCube(100);
        glPopMatrix();

        glColor3f(1.0, 0.27, 0.0);
        glPushMatrix();
        glTranslatef(-490, 238, -462);
        glScalef(0.04, 0.8, 0.1);
        glutSolidCube(100);
        glPopMatrix();

        glColor3f(1.0, 0.27, 0.0);
        glPushMatrix();
        glTranslatef(-490, 238, -412);
        glScalef(0.04, 0.8, 0.1);
        glutSolidCube(100);
        glPopMatrix();

        glColor3f(1.0, 0.27, 0.0);
        glPushMatrix();
        glTranslatef(-490, 238, -362);
        glScalef(0.04, 0.8, 0.1);
        glutSolidCube(100);
        glPopMatrix();


        glColor3f(1.0, 0.27, 0.0);
        glPushMatrix();
        glTranslatef(-490, 238, -312);
        glScalef(0.04, 0.8, 0.1);
        glutSolidCube(100);
        glPopMatrix();


        glColor3f(1.0, 0.27, 0.0);
        glPushMatrix();
        glTranslatef(-490, 238, -262);
        glScalef(0.04, 0.8, 0.1);
        glutSolidCube(100);
        glPopMatrix();
    }

    void pagarkanan4() {

     
        glColor3f(1.0, 0.27, 0.0);
        glPushMatrix();
        glTranslatef(486, 243, -515);
        glScalef(0.06, 0.05, 5.0);
        glutSolidCube(100);
        glPopMatrix();


        glColor3f(1.0, 0.27, 0.0);
        glPushMatrix();
        glTranslatef(486, 275, -515);
        glScalef(0.06, 0.05, 5.0);
        glutSolidCube(100);
        glPopMatrix();


        glColor3f(1.0, 0.27, 0.0);
        glPushMatrix();
        glTranslatef(490, 238, -762);
        glScalef(0.04, 0.8, 0.1);
        glutSolidCube(100);
        glPopMatrix();

        glColor3f(1.0, 0.27, 0.0);
        glPushMatrix();
        glTranslatef(490, 238, -712);
        glScalef(0.04, 0.8, 0.1);
        glutSolidCube(100);
        glPopMatrix();

        glColor3f(1.0, 0.27, 0.0);
        glPushMatrix();
        glTranslatef(490, 238, -662);
        glScalef(0.04, 0.8, 0.1);
        glutSolidCube(100);
        glPopMatrix();

        glColor3f(1.0, 0.27, 0.0);
        glPushMatrix();
        glTranslatef(490, 238, -612);
        glScalef(0.04, 0.8, 0.1);
        glutSolidCube(100);
        glPopMatrix();

        glColor3f(1.0, 0.27, 0.0);
        glPushMatrix();
        glTranslatef(490, 238, -562);
        glScalef(0.04, 0.8, 0.1);
        glutSolidCube(100);
        glPopMatrix();

        glColor3f(1.0, 0.27, 0.0);
        glPushMatrix();
        glTranslatef(490, 238, -512);
        glScalef(0.04, 0.8, 0.1);
        glutSolidCube(100);
        glPopMatrix();

        glColor3f(1.0, 0.27, 0.0);
        glPushMatrix();
        glTranslatef(490, 238, -462);
        glScalef(0.04, 0.8, 0.1);
        glutSolidCube(100);
        glPopMatrix();

        glColor3f(1.0, 0.27, 0.0);
        glPushMatrix();
        glTranslatef(490, 238, -412);
        glScalef(0.04, 0.8, 0.1);
        glutSolidCube(100);
        glPopMatrix();

        glColor3f(1.0, 0.27, 0.0);
        glPushMatrix();
        glTranslatef(490, 238, -362);
        glScalef(0.04, 0.8, 0.1);
        glutSolidCube(100);
        glPopMatrix();


        glColor3f(1.0, 0.27, 0.0);
        glPushMatrix();
        glTranslatef(490, 238, -312);
        glScalef(0.04, 0.8, 0.1);
        glutSolidCube(100);
        glPopMatrix();

        glColor3f(1.0, 0.27, 0.0);
        glPushMatrix();
        glTranslatef(490, 238, -262);
        glScalef(0.04, 0.8, 0.1);
        glutSolidCube(100);
        glPopMatrix();
    }

    void pagarbelakang4() {
    
        glColor3f(1.0, 0.27, 0.0);
        glPushMatrix();
        glTranslatef(6, 297, -600);
        glScalef(4.5, 0.05, 0.09);
        glutSolidCube(100);
        glPopMatrix();


        glColor3f(1.0, 0.27, 0.0);
        glPushMatrix();
        glTranslatef(6, 267, -600);
        glScalef(4.5, 0.05, 0.09);
        glutSolidCube(100);
        glPopMatrix();


        glColor3f(1.0, 0.27, 0.0);
        glPushMatrix();
        glTranslatef(229, 274, -600);
        glScalef(0.04, 0.5, 0.1);
        glutSolidCube(100);
        glPopMatrix();

        glColor3f(1.0, 0.27, 0.0);
        glPushMatrix();
        glTranslatef(188, 274, -600);
        glScalef(0.04, 0.5, 0.1);
        glutSolidCube(100);
        glPopMatrix();

        glColor3f(1.0, 0.27, 0.0);
        glPushMatrix();
        glTranslatef(147, 274, -600);
        glScalef(0.04, 0.5, 0.1);
        glutSolidCube(100);
        glPopMatrix();

        glColor3f(1.0, 0.27, 0.0);
        glPushMatrix();
        glTranslatef(106, 274, -600);
        glScalef(0.04, 0.5, 0.1);
        glutSolidCube(100);
        glPopMatrix();

        glColor3f(1.0, 0.27, 0.0);
        glPushMatrix();
        glTranslatef(65, 274, -600);
        glScalef(0.04, 0.5, 0.1);
        glutSolidCube(100);
        glPopMatrix();

        glColor3f(1.0, 0.27, 0.0);
        glPushMatrix();
        glTranslatef(24, 274, -600);
        glScalef(0.04, 0.5, 0.1);
        glutSolidCube(100);
        glPopMatrix();

        glColor3f(1.0, 0.27, 0.0);
        glPushMatrix();
        glTranslatef(-17, 274, -600);
        glScalef(0.04, 0.5, 0.1);
        glutSolidCube(100);
        glPopMatrix();

        glColor3f(1.0, 0.27, 0.0);
        glPushMatrix();
        glTranslatef(-58, 274, -600);
        glScalef(0.04, 0.5, 0.1);
        glutSolidCube(100);
        glPopMatrix();

        glColor3f(1.0, 0.27, 0.0);
        glPushMatrix();
        glTranslatef(-99, 274, -600);
        glScalef(0.04, 0.5, 0.1);
        glutSolidCube(100);
        glPopMatrix();

        glColor3f(1.0, 0.27, 0.0);
        glPushMatrix();
        glTranslatef(-140, 274, -600);
        glScalef(0.04, 0.5, 0.1);
        glutSolidCube(100);
        glPopMatrix();

        glColor3f(1.0, 0.27, 0.0);
        glPushMatrix();
        glTranslatef(-181, 274, -600);
        glScalef(0.04, 0.5, 0.1);
        glutSolidCube(100);
        glPopMatrix();

        glColor3f(1.0, 0.27, 0.0);
        glPushMatrix();
        glTranslatef(-222, 274, -600);
        glScalef(0.04, 0.5, 0.1);
        glutSolidCube(100);
        glPopMatrix();

    }

    void tower() {
        //kotak_tower
        glColor3f(0.16, 0.27, 0.16);
        glPushMatrix();
        glTranslatef(4, 1652, -231.5);
        glScalef(2, 0.8, 2);
        glutSolidCube(100);
        glPopMatrix();

        //bulat_tower
        glColor3f(0.16, 0.27, 0.16);
        glPushMatrix();
        glTranslatef(4, 1652, -231.5);
        glutSolidSphere(50, 60, 60);
        glPopMatrix();

        //menara
        glColor3f(0.16, 0.27, 0.16);
        glPushMatrix();
        glTranslatef(4, 1652, -231.5);
        glScalef(0.15, 10, 0.15);
        glutSolidCube(100);
        glPopMatrix();
    }

    void dinding() {
        //pintu 1
        glColor3f(1.0, 0.27, 0.0);
        glPushMatrix();
        glScalef(1.5, 2.2, 0.1);
        glutSolidCube(100);
        glPopMatrix();

        //pintu3
        glColor3f(1.0, 0.27, 0.0);
        glPushMatrix();
        glTranslatef(-4, 516, -80);
        glScalef(0.9, 1.1, 0.1);
        glutSolidCube(100);
        glPopMatrix();

        //pintu2
        glColor3f(1.0, 0.27, 0.0);
        glPushMatrix();
        glTranslatef(-5, 273, -118);
        glScalef(1.3, 1.5, 0.1);
        glutSolidCube(100);
        glPopMatrix();

        //pintu 4
        glColor3f(1.0, 0.27, 0.0);
        glPushMatrix();
        glTranslatef(-5, -7, -775);
        glScalef(1.5, 2.2, 0.1);
        glutSolidCube(100);
        glPopMatrix();

        //pintu 5
        glColor3f(1.0, 0.27, 0.0);
        glPushMatrix();
        glTranslatef(-5, 280 , -702 );
        glScalef(1.0, 1.2, 0.1);
        glutSolidCube(100);
        glPopMatrix();

        //pintu 6
        glColor3f(1.0, 0.27, 0.0);
        glPushMatrix();
        glTranslatef(12, 512, -714);
        glScalef(0.9, 1.1, 0.1);
        glutSolidCube(100);
        glPopMatrix();


        glColor3f(1.0, 0.27, 0.0);
        glPushMatrix();
        glTranslatef(232, 86, 18);
        glScalef(1.2, 0.10, 0.1);
        glutSolidCube(100);
        glPopMatrix();

        glColor3f(1.0, 0.27, 0.0);
        glPushMatrix();
        glTranslatef(232, -5, 18);
        glScalef(1.2, 0.10, 0.1);
        glutSolidCube(100);
        glPopMatrix();

        glColor3f(1.0, 0.27, 0.0);
        glPushMatrix();
        glTranslatef(232, 37, 18);
        glScalef(1.2, 0.10, 0.1);
        glutSolidCube(100);
        glPopMatrix();

        glColor3f(1.0, 0.27, 0.0);
        glPushMatrix();
        glTranslatef(176, 42, 8);
        glScalef(0.1, 1.0, 0.1);
        glutSolidCube(100);
        glPopMatrix();

        glColor3f(1.0, 0.27, 0.0);
        glPushMatrix();
        glTranslatef(292, 42, 8);
        glScalef(0.1, 1.0, 0.1);
        glutSolidCube(100);
        glPopMatrix();

        glColor3f(1.0, 0.27, 0.0);
        glPushMatrix();
        glTranslatef(237, 42, 8);
        glScalef(0.1, 1.0, 0.1);
        glutSolidCube(100);
        glPopMatrix();

    }

    void jendela() {
        glColor3f(1.0, 0.27, 0.0);
        glPushMatrix();
        glTranslatef(232, 86, 18);
        glScalef(1.2, 0.10, 0.1);
        glutSolidCube(100);
        glPopMatrix();

        glColor3f(1.0, 0.27, 0.0);
        glPushMatrix();
        glTranslatef(232, -5, 18);
        glScalef(1.2, 0.10, 0.1);
        glutSolidCube(100);
        glPopMatrix();

        glColor3f(1.0, 0.27, 0.0);
        glPushMatrix();
        glTranslatef(232, 37, 18);
        glScalef(1.2, 0.10, 0.1);
        glutSolidCube(100);
        glPopMatrix();

        glColor3f(1.0, 0.27, 0.0);
        glPushMatrix();
        glTranslatef(176, 42, 8);
        glScalef(0.1, 1.0, 0.1);
        glutSolidCube(100);
        glPopMatrix();

        glColor3f(1.0, 0.27, 0.0);
        glPushMatrix();
        glTranslatef(292, 42, 8);
        glScalef(0.1, 1.0, 0.1);
        glutSolidCube(100);
        glPopMatrix();

        glColor3f(1.0, 0.27, 0.0);
        glPushMatrix();
        glTranslatef(237, 42, 8);
        glScalef(0.1, 1.0, 0.1);
        glutSolidCube(100);
        glPopMatrix();
    }

    void jendela1() {
        glColor3f(1.0, 0.27, 0.0);
        glPushMatrix();
        glTranslatef(232, 86, 18);
        glScalef(1.2, 0.10, 0.1);
        glutSolidCube(100);
        glPopMatrix();

        glColor3f(1.0, 0.27, 0.0);
        glPushMatrix();
        glTranslatef(232, -5, 18);
        glScalef(1.2, 0.10, 0.1);
        glutSolidCube(100);
        glPopMatrix();

        glColor3f(1.0, 0.27, 0.0);
        glPushMatrix();
        glTranslatef(232, 37, 18);
        glScalef(1.2, 0.10, 0.1);
        glutSolidCube(100);
        glPopMatrix();

        glColor3f(1.0, 0.27, 0.0);
        glPushMatrix();
        glTranslatef(176, 42, 8);
        glScalef(0.1, 1.0, 0.1);
        glutSolidCube(100);
        glPopMatrix();

        glColor3f(1.0, 0.27, 0.0);
        glPushMatrix();
        glTranslatef(292, 42, 8);
        glScalef(0.1, 1.0, 0.1);
        glutSolidCube(100);
        glPopMatrix();

        glColor3f(1.0, 0.27, 0.0);
        glPushMatrix();
        glTranslatef(237, 42, 8);
        glScalef(0.1, 1.0, 0.1);
        glutSolidCube(100);
        glPopMatrix();
    }

    void jendela2() {
        glColor3f(1.0, 0.27, 0.0);
        glPushMatrix();
        glTranslatef(232, 86, 18);
        glScalef(1.2, 0.10, 0.1);
        glutSolidCube(100);
        glPopMatrix();

        glColor3f(1.0, 0.27, 0.0);
        glPushMatrix();
        glTranslatef(232, -5, 18);
        glScalef(1.2, 0.10, 0.1);
        glutSolidCube(100);
        glPopMatrix();

        glColor3f(1.0, 0.27, 0.0);
        glPushMatrix();
        glTranslatef(232, 37, 18);
        glScalef(1.2, 0.10, 0.1);
        glutSolidCube(100);
        glPopMatrix();

        glColor3f(1.0, 0.27, 0.0);
        glPushMatrix();
        glTranslatef(176, 42, 8);
        glScalef(0.1, 1.0, 0.1);
        glutSolidCube(100);
        glPopMatrix();

        glColor3f(1.0, 0.27, 0.0);
        glPushMatrix();
        glTranslatef(292, 42, 8);
        glScalef(0.1, 1.0, 0.1);
        glutSolidCube(100);
        glPopMatrix();

        glColor3f(1.0, 0.27, 0.0);
        glPushMatrix();
        glTranslatef(237, 42, 8);
        glScalef(0.1, 1.0, 0.1);
        glutSolidCube(100);
        glPopMatrix();
    }

    void jendela3() {
        glColor3f(1.0, 0.27, 0.0);
        glPushMatrix();
        glTranslatef(232, 86, 18);
        glScalef(1.2, 0.10, 0.1);
        glutSolidCube(100);
        glPopMatrix();

        glColor3f(1.0, 0.27, 0.0);
        glPushMatrix();
        glTranslatef(232, -5, 18);
        glScalef(1.2, 0.10, 0.1);
        glutSolidCube(100);
        glPopMatrix();

        glColor3f(1.0, 0.27, 0.0);
        glPushMatrix();
        glTranslatef(232, 37, 18);
        glScalef(1.2, 0.10, 0.1);
        glutSolidCube(100);
        glPopMatrix();

        glColor3f(1.0, 0.27, 0.0);
        glPushMatrix();
        glTranslatef(176, 42, 8);
        glScalef(0.1, 1.0, 0.1);
        glutSolidCube(100);
        glPopMatrix();

        glColor3f(1.0, 0.27, 0.0);
        glPushMatrix();
        glTranslatef(292, 42, 8);
        glScalef(0.1, 1.0, 0.1);
        glutSolidCube(100);
        glPopMatrix();

        glColor3f(1.0, 0.27, 0.0);
        glPushMatrix();
        glTranslatef(237, 42, 8);
        glScalef(0.1, 1.0, 0.1);
        glutSolidCube(100);
        glPopMatrix();
    }

    void jendela4() {
        glColor3f(1.0, 0.27, 0.0);
        glPushMatrix();
        glTranslatef(232, 86, 18);
        glScalef(1.2, 0.10, 0.1);
        glutSolidCube(100);
        glPopMatrix();

        glColor3f(1.0, 0.27, 0.0);
        glPushMatrix();
        glTranslatef(232, -5, 18);
        glScalef(1.2, 0.10, 0.1);
        glutSolidCube(100);
        glPopMatrix();

        glColor3f(1.0, 0.27, 0.0);
        glPushMatrix();
        glTranslatef(232, 37, 18);
        glScalef(1.2, 0.10, 0.1);
        glutSolidCube(100);
        glPopMatrix();

        glColor3f(1.0, 0.27, 0.0);
        glPushMatrix();
        glTranslatef(176, 42, 8);
        glScalef(0.1, 1.0, 0.1);
        glutSolidCube(100);
        glPopMatrix();

        glColor3f(1.0, 0.27, 0.0);
        glPushMatrix();
        glTranslatef(292, 42, 8);
        glScalef(0.1, 1.0, 0.1);
        glutSolidCube(100);
        glPopMatrix();

        glColor3f(1.0, 0.27, 0.0);
        glPushMatrix();
        glTranslatef(237, 42, 8);
        glScalef(0.1, 1.0, 0.1);
        glutSolidCube(100);
        glPopMatrix();
    }

    void tangga() {
        glColor3f(0.12, 0.12, 0.12);
        glPushMatrix();
        glTranslatef(0, 29, 15);
        glScalef(2.0, 3.4, 0.3);
        glutSolidCube(100);
        glPopMatrix();

        glColor3f(0.12, 0.12, 0.12);
        glPushMatrix();
        glTranslatef(0, 29, 45);
        glScalef(2.0, 2.8, 0.3);
        glutSolidCube(100);
        glPopMatrix();

        glColor3f(0.12, 0.12, 0.12);
        glPushMatrix();
        glTranslatef(0, 29, 75);
        glScalef(2.0, 2.2, 0.3);
        glutSolidCube(100);
        glPopMatrix();

        glColor3f(0.12, 0.12, 0.12);
        glPushMatrix();
        glTranslatef(0, 29, 105);
        glScalef(2.0, 1.6, 0.3);
        glutSolidCube(100);
        glPopMatrix();

        glColor3f(0.12, 0.12, 0.12);
        glPushMatrix();
        glTranslatef(0, 29, 135);
        glScalef(2.0, 1.0, 0.3);
        glutSolidCube(100);
        glPopMatrix();

        glColor3f(0.12, 0.12, 0.12);
        glPushMatrix();
        glTranslatef(0, 29, 165);
        glScalef(2.0, 0.6, 0.3);
        glutSolidCube(100);
        glPopMatrix();

        glColor3f(0.12, 0.12, 0.12);
        glPushMatrix();
        glTranslatef(0, 20, 195);
        glScalef(2.0, 0.4, 0.3);
        glutSolidCube(100);
        glPopMatrix();


  
     
    }


void draw() {
    // Mulai tuliskan isi pikiranmu disini

    glPushMatrix();
    glTranslatef(0,0,-499);
    lantai();
    glPopMatrix();

    glPushMatrix();
    glTranslatef(0, 163, -438);
    lantai1();
    glPopMatrix();

    glPushMatrix();
    glTranslatef(0, -86, -615);
    atap();
    glPopMatrix();

    glPushMatrix();
    glTranslatef(0, 0, 0);
    atap_kotak();
    glPopMatrix();
    
    //lantai2
    glPushMatrix();
    glTranslatef(0, 238, -499);
    lantai2();
    glPopMatrix();

    glPushMatrix();
    glTranslatef(0, 182, -532);
    atap1();
    glPopMatrix();

    //lantai3
    glPushMatrix();
    glTranslatef(-2, 470, -526);
    lantai3();
    glPopMatrix();

    glPushMatrix();
    glTranslatef(2, 376, -534);
    atap2();
    glPopMatrix();

    //lantai4
    glPushMatrix();
    glTranslatef(-2, 166, -623);
    lantai4();
    glPopMatrix();

    glPushMatrix();
    glTranslatef(-19, 535, -556);
    atap3();
    glPopMatrix();

    pagardepan1();
    pagarkiri1();
    pagarkanan1();
    pagarbelakang1();

    glPushMatrix();
    glTranslatef(0, 318 , -27);
    pagardepan2();
    glPopMatrix();

    glPushMatrix();
    glTranslatef(0, 318, -27);
    pagarkiri2();
    glPopMatrix();

    glPushMatrix();
    glTranslatef(0, 318, -27);
    pagarkanan2();
    glPopMatrix();

    glPushMatrix();
    glTranslatef(0, 318, 27);
    pagarbelakang2();
    glPopMatrix();
   
    glPushMatrix();
    glTranslatef(0, -175, 0);
    dasar1();
    glPopMatrix();

    glPushMatrix();
    glTranslatef(0, 550, -51);
    pagardepan3();
    glPopMatrix();

    glPushMatrix();
    glTranslatef(139, 564, -7);
    pagarkiri3();
    glPopMatrix();

    glPushMatrix();
    glTranslatef(-139, 564, -7);
    pagarkanan3();
    glPopMatrix();

    glPushMatrix();
    glTranslatef(1, 547, -7);
    pagarbelakang3();
    glPopMatrix();
 
    glPushMatrix();
    glTranslatef(0, 23, 0);
    dasar2();
    glPopMatrix();

    glPushMatrix();
    glTranslatef(0, 724, -203);
    pagardepan4();
    glPopMatrix();
    

    glPushMatrix();            
    glTranslatef(268, 745, -13);
    pagarkiri4();
    glPopMatrix();

    glPushMatrix();
    glTranslatef(-256, 745, -13);
    pagarkanan4();
    glPopMatrix();
    
    glPushMatrix();
    glTranslatef(0, 724, -176);
    pagarbelakang4();
    glPopMatrix();

    glPushMatrix();
    glTranslatef(-13,-411, -293);
    tower();
    glPopMatrix();

    glPushMatrix();
    glTranslatef(-5, 321, -118);
    dinding();
    glPopMatrix();

    glPushMatrix();
    glTranslatef(-455, 323, -117);
    jendela();
    glPopMatrix();

    glPushMatrix();
    glTranslatef(-455, 641, -229);
    jendela1();
    glPopMatrix();

    glPushMatrix();
    glTranslatef(-38, 641, -229);
    jendela2();
    glPopMatrix();

    glPushMatrix();
    glTranslatef(-38, 832, -218);
    jendela3();
    glPopMatrix();

    glPushMatrix();
    glTranslatef(-426, 832, -218);
    jendela4();
    glPopMatrix();

    glPushMatrix();

    tangga();
    glPopMatrix();
    

    ground();
    cout << "X_GESER = " << x_geser << "  Y_GESER = " << y_geser << " Z_GESER = " << z_geser << endl;
    glFlush();

}

void display() {
    glClear(GL_COLOR_BUFFER_BIT | GL_DEPTH_BUFFER_BIT);
    glLoadIdentity();
    camera();
    draw();

    glutSwapBuffers();
}

void reshape(int w, int h) {
    glViewport(0, 0, w, h);
    glMatrixMode(GL_PROJECTION);
    glLoadIdentity();
    gluPerspective(50, 16.0 / 9.0, 2, 10000);
    glMatrixMode(GL_MODELVIEW);
}

void timer(int) {
    glutPostRedisplay();
    glutWarpPointer(width / 2, height / 2);
    glutTimerFunc(1000 / FPS, timer, 0);
}

void passive_motion(int x, int y) {
    int dev_x, dev_y;
    dev_x = (width / 2) - x;
    dev_y = (height / 2) - y;
    yaw += (float)dev_x / 20.0;
    pitch += (float)dev_y / 20.0;
}

void camera() {
    if (motion.Forward) {
        camX += cos((yaw + 90) * TO_RADIANS) * 2;
        camZ -= sin((yaw + 90) * TO_RADIANS) * 2;
    }
    if (motion.Backward) {
        camX += cos((yaw + 90 + 180) * TO_RADIANS) * 2;
        camZ -= sin((yaw + 90 + 180) * TO_RADIANS) * 2;
    }
    if (motion.Left) {
        camX += cos((yaw + 90 + 90) * TO_RADIANS) * 2;
        camZ -= sin((yaw + 90 + 90) * TO_RADIANS) * 2;
    }
    if (motion.Right) {
        camX += cos((yaw + 90 - 90) * TO_RADIANS) * 2;
        camZ -= sin((yaw + 90 - 90) * TO_RADIANS) * 2;
    }
    if (motion.Naik) {
        terbang -= 2.0;
    }
    if (motion.Turun) {
        terbang += 2.0;
    }

    if (pitch >= 70)
        pitch = 70;
    if (pitch <= -90)
        pitch = -90;


    glRotatef(-pitch, 1.0, 0.0, 0.0);
    glRotatef(-yaw, 0.0, 1.0, 0.0);

    glTranslatef(-camX, -terbang, -350 - camZ);
    if (terbang < 25)
        terbang = 24;
}

void keyboard(unsigned char key, int x, int y) {
    switch (key) {
    case 'W':
    case 'w':
        motion.Forward = true;
        break;
    case 'A':
    case 'a':
        motion.Left = true;
        break;
    case 'S':
    case 's':
        motion.Backward = true;
        break;
    case 'D':
    case 'd':
        motion.Right = true;
        break;
    case 'E':
    case 'e':
        motion.Naik = true;
        break;
    case 'Q':
    case 'q':
        motion.Turun = true;
        break;

    case 'l':
        x_geser += 1.0;
        break;
    case 'j':
        x_geser -= 1.0;
        break;
    case '=':
        y_geser += 1.0;
        break;
    case '-':
        y_geser -= 1.0;
        break;
    case 'i':
        z_geser -= 1.0;
        break;
    case 'k':
        z_geser += 1.0;
        break;
    case '`': // KELUAR DARI PROGRAM
        exit(1);
    }
}

void keyboard_up(unsigned char key, int x, int y) {
    switch (key) {
    case 'W':
    case 'w':
        motion.Forward = false;
        break;
    case 'A':
    case 'a':
        motion.Left = false;
        break;
    case 'S':
    case 's':
        motion.Backward = false;
        break;
    case 'D':
    case 'd':
        motion.Right = false;
        break;
    case 'E':
    case 'e':
        motion.Naik = false;
        break;
    case 'Q':
    case 'q':
        motion.Turun = false;
        break;
    }
}

int main(int argc, char** argv) {
    glutInit(&argc, argv);
    glutInitDisplayMode(GLUT_DOUBLE | GLUT_RGB | GLUT_DEPTH);
    glutInitWindowSize(width, height);
    glutCreateWindow("TR GRAFKOM KELOMPOK X");

    init();
    glutDisplayFunc(display);
    glutReshapeFunc(reshape);
    glutPassiveMotionFunc(passive_motion);
    glutTimerFunc(0, timer, 0);
    glutKeyboardFunc(keyboard);
    glutKeyboardUpFunc(keyboard_up);

    glutMainLoop();
    return 0;
}
