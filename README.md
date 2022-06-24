#include <GL/glut.h>
#include <math.h>
#include <iostream>
using namespace std;

#define FPS 120
#define TO_RADIANS 3.14/180.0

//  Anggota Kelompok X
//  Nama (NIM)
//  Nama (NIM)
//  Nama (NIM)
//  Nama (NIM)
//  Nama (NIM)

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
        glColor3f(1.0f, 1.0f, 1.0f);
        glBegin(GL_POLYGON);
        glVertex3f(-500, 200, -200);
        glVertex3f(-650, 0, -200);
        glVertex3f(650, 0, -200);
        glVertex3f(500, 200, -200);
        glEnd();
        glPopMatrix();

        //belakang
        glPushMatrix();
        glColor3f(1.0f, 1.0f, 1.0f);
        glBegin(GL_POLYGON);
        glVertex3f(-500, 200, 500);
        glVertex3f(-650, 0, 500);
        glVertex3f(650, 0, 500);
        glVertex3f(500, 200, 500);
        glEnd();
        glPopMatrix();

        //kiri
        glPushMatrix();
        glColor3f(1.0f, 1.0f, 1.0f);
        glBegin(GL_POLYGON);
        glVertex3f(-500, 200, -500);
        glVertex3f(-650, 0, -500);
        glVertex3f(-650, 0, 500);
        glVertex3f(-500, 200, 500);
        glEnd();
        glPopMatrix();

        //KANAN
        glPushMatrix();
        glColor3f(1.0f, 1.0f, 1.0f);
        glBegin(GL_POLYGON);
        glVertex3f(500, 200, -500);
        glVertex3f(650, 0, -500);
        glVertex3f(650, 0, 500);
        glVertex3f(500, 200, 500);
        glEnd();
        glPopMatrix();

        //atas
        glPushMatrix();
        glColor3f(1.0f, 1.0f, 1.0f);
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
    glColor3f(0.0f, 0.0f, 0.0f);
    glBegin(GL_POLYGON);
    glVertex3f(-400, 50, 100);
    glVertex3f(-400, 300, 100);
    glVertex3f(400, 300, 100);
    glVertex3f(400, 50, 100);
    glEnd();
    glPopMatrix();

    //belakang
    glPushMatrix();
    glColor3f(0.0f, 0.0f, 0.0f);
    glBegin(GL_POLYGON);
    glVertex3f(-400, 50, -450);
    glVertex3f(-400, 300, -450);
    glVertex3f(400, 300, -450);
    glVertex3f(400, 50, -450);
    glEnd();
    glPopMatrix();

    //kiri
    glPushMatrix();
    glColor3f(0.0f, 0.0f, 0.0f);
    glBegin(GL_POLYGON);
    glVertex3f(-400, 50, -450);
    glVertex3f(-400, 300, -450);
    glVertex3f(-400, 300, 325);
    glVertex3f(-400, 50, 325);
    glEnd();
    glPopMatrix();

    //KANAN
    glPushMatrix();
    glColor3f(0.0f, 0.0f, 0.0f);
    glBegin(GL_POLYGON);
    glVertex3f(400, 50, -450);
    glVertex3f(400, 300, -450);
    glVertex3f(400, 300, 325);
    glVertex3f(400, 50, 325);
    glEnd();
    glPopMatrix();

    //atas
    glPushMatrix();
    glColor3f(1.0f, 1.0f, 1.0f);
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
    glColor3f(1.0f, 1.0f, 1.0f);
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
    glColor3f(1.0f, 0.0f, 1.0f);
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
    glColor3f(0.0f, 1.0f, 0.0f);
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
    glColor3f(1.0f, 0.0f, 0.0f);
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
    glColor3f(1.0f, 0.0f, 0.0f);
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
    glColor3f(0.0f, 1.0f, 0.0f);
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
    glColor3f(0.0f, 0.0f, 1.0f);
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
    glColor3f(1.0f, 1.0f, 0.0f);
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
    glColor3f(1.0f, 1.0f, 1.0f);
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
    glColor3f(0.0f, 0.0f, 1.0f);
    glBegin(GL_POLYGON);
    glVertex3f(-350, 500, 100);
    glVertex3f(-350, 300, 100);
    glVertex3f(350, 300, 100);
    glVertex3f(350, 500, 100);
    glEnd();
    glPopMatrix();

    //belakang
    glPushMatrix();
    glColor3f(0.0f, 0.0f, 1.0f);
    glBegin(GL_POLYGON);
    glVertex3f(-350, 500, -400);
    glVertex3f(-350, 300, -400);
    glVertex3f(350, 300, -400);
    glVertex3f(350, 500, -400);
    glEnd();
    glPopMatrix();

    //kiri
    glPushMatrix();
    glColor3f(0.0f, 0.0f, 1.0f);
    glBegin(GL_POLYGON);
    glVertex3f(-350, 500, -400);
    glVertex3f(-350, 300, -400);
    glVertex3f(-350, 300, 330);
    glVertex3f(-350, 500, 330);
    glEnd();
    glPopMatrix();

    //KANAN
    glPushMatrix();
    glColor3f(0.0f, 0.0f, 1.0f);
    glBegin(GL_POLYGON);
    glVertex3f(350, 500, -400);
    glVertex3f(350, 300, -400);
    glVertex3f(350, 300, 330);
    glVertex3f(350, 500, 330);
    glEnd();
    glPopMatrix();

    //atas
    glPushMatrix();
    glColor3f(0.0f, 0.0f, 0.0f);
    glBegin(GL_POLYGON);
    glVertex3f(-350, 500, -400);
    glVertex3f(350, 500, -400);
    glVertex3f(350, 500, 330);
    glVertex3f(-350, 500, 330);
    glEnd();
    glPopMatrix();
}

void atap1() {
    //atap
 //atas 
    glPushMatrix();
    glTranslatef(0, 250.5, 0);
    glColor3f(0.0f, 1.0f, 1.0f);
    glBegin(GL_POLYGON);
    glVertex3f(-450, 300, -500);
    glVertex3f(450, 300, -500);
    glVertex3f(450, 300, 500);
    glVertex3f(-450, 300, 500);
    glEnd();
    glPopMatrix();

    //kanan
    glPushMatrix();
    glTranslatef(0, 500, 0);
    glColor3f(0.0f, 1.0f, 0.0f);
    glBegin(GL_POLYGON);
    glVertex3f(450, 50, -500);
    glVertex3f(300, 120, -170);
    glVertex3f(300, 120, 170);
    glVertex3f(450, 50, 500); glEnd();
    glPopMatrix();

    //kiri 
    glPushMatrix();
    glTranslatef(0, 500, 0);
    glColor3f(1.0f, 0.0f, 1.0f);
    glBegin(GL_POLYGON);
    glVertex3f(-450, 50, -500);
    glVertex3f(-300, 120, -170);
    glVertex3f(-300, 120, 170);
    glVertex3f(-450, 50, 500);
    glEnd();
    glPopMatrix();

    //belakang 
    glPushMatrix();
    glTranslatef(0, 500, 0);
    glColor3f(1.0f, 0.0f, 0.0f);
    glBegin(GL_POLYGON);
    glVertex3f(-450, 50, -500);
    glVertex3f(-300, 120, -170);
    glVertex3f(300, 120, -170);
    glVertex3f(450, 50, -500);
    glEnd(); 
    glPopMatrix();

    //depan 
    glPushMatrix();
    glTranslatef(0, 500, 0);
    glColor3f(1.0f, 1.0f, 1.0f);
    glBegin(GL_POLYGON);
    glVertex3f(-450, 50, 500);
    glVertex3f(-300, 120, 170);
    glVertex3f(300, 120, 170);
    glVertex3f(450, 50, 500);
    glEnd();
    glPopMatrix();
}

void lantai3() {
    //lantai 3
    //dasar
    glPushMatrix();
    glTranslatef(0, 0, 224);
    glColor3f(0.0f, 0.0f, 1.0f);
    glBegin(GL_POLYGON);
    glVertex3f(-300, 480, 100);
    glVertex3f(-300, 300, 100);
    glVertex3f(300, 300, 100);
    glVertex3f(300, 480, 100);
    glEnd();
    glPopMatrix();

    //belakang
    glPushMatrix();
    glColor3f(0.0f, 0.0f, 1.0f);
    glBegin(GL_POLYGON);
    glVertex3f(-300, 480, -300);
    glVertex3f(-300, 300, -300);
    glVertex3f(300, 300, -300);
    glVertex3f(300, 480, -300);
    glEnd();
    glPopMatrix();

    //kiri
    glPushMatrix();
    glColor3f(0.0f, 0.0f, 1.0f);
    glBegin(GL_POLYGON);
    glVertex3f(-300, 480, -300);
    glVertex3f(-300, 300, -300);
    glVertex3f(-300, 300, 330);
    glVertex3f(-300, 480, 330);
    glEnd();
    glPopMatrix();

    //KANAN
    glPushMatrix();
    glColor3f(0.0f, 0.0f, 1.0f);
    glBegin(GL_POLYGON);
    glVertex3f(300, 480, -300);
    glVertex3f(300, 300, -300);
    glVertex3f(300, 300, 330);
    glVertex3f(300, 480, 330);
    glEnd();
    glPopMatrix();

    //atas
    glPushMatrix();
    glColor3f(0.0f, 0.0f, 0.0f);
    glBegin(GL_POLYGON);
    glVertex3f(-300, 480, -300);
    glVertex3f(300, 480, -300);
    glVertex3f(300, 480, 330);
    glVertex3f(-300, 480, 330);
    glEnd();
    glPopMatrix();
}

void atap2() {
    //atap lantai 1
       //atas 
    glPushMatrix();
    glTranslatef(0, 250.5, 0);
    glColor3f(0.0f, 1.0f, 1.0f);
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
    glColor3f(0.0f, 1.0f, 0.0f);
    glBegin(GL_POLYGON);
    glVertex3f(400, 50, -500);
    glVertex3f(200, 100, -170);
    glVertex3f(200, 100, 170);
    glVertex3f(400, 50, 500); glEnd();
    glPopMatrix();

    //kiri 
    glPushMatrix();
    glTranslatef(0, 500, 0);
    glColor3f(1.0f, 0.0f, 1.0f);
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
    glColor3f(1.0f, 0.0f, 0.0f);
    glBegin(GL_POLYGON);
    glVertex3f(-400, 50, -500);
    glVertex3f(-200, 100, -170);
    glVertex3f(200, 100, -170);
    glVertex3f(400, 50, -500);
    glEnd(); glPopMatrix();

    //depan 
    glPushMatrix();
    glTranslatef(0, 500, 0);
    glColor3f(1.0f, 1.0f, 1.0f);
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
    glColor3f(0.0f, 1.0f, 1.0f);
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
    glColor3f(0.0f, 0.0f, 1.0f);
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
    glColor3f(0.0f, 0.0f, 1.0f);
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
    glColor3f(0.0f, 0.0f, 1.0f);
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
    glColor3f(1.0f, 0.0f, 0.0f);
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
    glColor3f(0.0f, 1.0f, 0.0f);
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
    glColor3f(0.0f, 1.0f, 1.0f);
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
    glColor3f(0.0f, 1.0f, 0.0f);
    glBegin(GL_POLYGON);
    glVertex3f(350, 50, -400);
    glVertex3f(175, 200, -170);
    glVertex3f(175, 200, 170);
    glVertex3f(350, 50, 400); glEnd();
    glPopMatrix();

    //kiri 
    glPushMatrix();
    glTranslatef(0, 500, 0);
    glColor3f(1.0f, 0.0f, 1.0f);
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
    glColor3f(1.0f, 0.0f, 0.0f);
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
    glColor3f(1.0f, 1.0f, 1.0f);
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
    glColor3f(1.0f, 1.0f, 1.0f);
    glBegin(GL_POLYGON);
    glVertex3f(-175, 50, 170);
    glVertex3f(-175, 50, -170);
    glVertex3f(175, 50, -170);
    glVertex3f(175, 50, 170);
    glEnd();
    glPopMatrix();
}


void pagar_lantai1() {

}


void pagar_lantai1depan() {
    //DEPAN
    //pagar atas 1 kanan
    glColor3f(0.68, 0.1, 0.1);
    glPushMatrix();
    glTranslatef(283, 297, 4);
    glScalef(3.5, 0.10, 0.2);
    glutSolidCube(100);
    glPopMatrix();

    //pagar bawah 1 kanan
    glColor3f(0.68, 0.1, 0.1);
    glPushMatrix();
    glTranslatef(283, 245, 4);
    glScalef(3.5, 0.10, 0.2);
    glutSolidCube(100);
    glPopMatrix();

    //pagar atas 2 kiri
    glColor3f(0.68, 0.1, 0.1);
    glPushMatrix();
    glTranslatef(-283, 297, 4);
    glScalef(3.5, 0.10, 0.2);
    glutSolidCube(100);
    glPopMatrix();

    //pagar bawah 2 kiri
    glColor3f(0.68, 0.1, 0.1);
    glPushMatrix();
    glTranslatef(-283, 245, 4);
    glScalef(3.5, 0.10, 0.2);
    glutSolidCube(100);
    glPopMatrix();

    //CAGAK
    //CAGAK merah kanan  Depan
    glColor3f(0.68, 0.1, 0.1);
    glPushMatrix();
    glTranslatef(113, 248, -5);
    glScalef(0.2, 1.0, 0.1);
    glutSolidCube(100);
    glPopMatrix();

    glColor3f(0.68, 0.1, 0.1);
    glPushMatrix();
    glTranslatef(231, 248, -5);
    glScalef(0.2, 1.0, 0.1);
    glutSolidCube(100);
    glPopMatrix();

    glColor3f(0.68, 0.1, 0.1);
    glPushMatrix();
    glTranslatef(344, 248, -5);
    glScalef(0.2, 1.0, 0.1);
    glutSolidCube(100);
    glPopMatrix();

    glColor3f(0.68, 0.1, 0.1);
    glPushMatrix();
    glTranslatef(457, 248, -5);
    glScalef(0.2, 1.0, 0.1);
    glutSolidCube(100);
    glPopMatrix();

    //CAGAK merah kiri Depan
    glColor3f(0.68, 0.1, 0.1);
    glPushMatrix();
    glTranslatef(-457, 248, 5);
    glScalef(0.2, 1.0, 0.1);
    glutSolidCube(100);
    glPopMatrix();

    glColor3f(0.68, 0.1, 0.1);
    glPushMatrix();
    glTranslatef(-344, 248, 5);
    glScalef(0.2, 1.0, 0.1);
    glutSolidCube(100);
    glPopMatrix();

    glColor3f(0.68, 0.1, 0.1);
    glPushMatrix();
    glTranslatef(-231, 248, 5);
    glScalef(0.2, 1.0, 0.1);
    glutSolidCube(100);
    glPopMatrix();

    glColor3f(0.68, 0.1, 0.1);
    glPushMatrix();
    glTranslatef(-113, 248, 5);
    glScalef(0.2, 1.0, 0.1);
    glutSolidCube(100);
    glPopMatrix();
}

void pagar_lantai1belakang() {
    //DEPAN
    //pagar atas 1 kanan
    glColor3f(0.68, 0.1, 0.1);
    glPushMatrix();
    glTranslatef(283, 297, -989);
    glScalef(3.5, 0.10, 0.2);
    glutSolidCube(100);
    glPopMatrix();


    //pagar bawah 1 kanan
    glColor3f(0.68, 0.1, 0.1);
    glPushMatrix();
    glTranslatef(283, 245, -989);
    glScalef(3.5, 0.10, 0.2);
    glutSolidCube(100);
    glPopMatrix();

    //pagar atas 2 kiri
    glColor3f(0.68, 0.1, 0.1);
    glPushMatrix();
    glTranslatef(-283, 297, -989);
     glScalef(3.5, 0.10, 0.2);
    glutSolidCube(100);
    glPopMatrix();

    //pagar bawah 2 kiri
    glColor3f(0.68, 0.1, 0.1);
    glPushMatrix();
    glTranslatef(-283, 245, -989);
    glScalef(3.5, 0.10, 0.2);
    glutSolidCube(100);
    glPopMatrix();

    //CAGAK
    //CAGAK merah kanan  Depan
    glColor3f(0.68, 0.1, 0.1);
    glPushMatrix();
    glTranslatef(113, 248, -989);
    glScalef(0.2, 1.0, 0.1);
    glutSolidCube(100);
    glPopMatrix();

    glColor3f(0.68, 0.1, 0.1);
    glPushMatrix();
    glTranslatef(231, 248, -989);
    glScalef(0.2, 1.0, 0.1);
    glutSolidCube(100);
    glPopMatrix();

    glColor3f(0.68, 0.1, 0.1);
    glPushMatrix();
    glTranslatef(344, 248, -989);
    glScalef(0.2, 1.0, 0.1);
    glutSolidCube(100);
    glPopMatrix();

    glColor3f(0.68, 0.1, 0.1);
    glPushMatrix();
    glTranslatef(457, 248, -989);
    glScalef(0.2, 1.0, 0.1);
    glutSolidCube(100);
    glPopMatrix();

    //CAGAK merah kiri Depan
    glColor3f(0.68, 0.1, 0.1);
    glPushMatrix();
    glTranslatef(-113, 248, -989);
    glScalef(0.2, 1.0, 0.1);
    glutSolidCube(100);
    glPopMatrix();

    glColor3f(0.68, 0.1, 0.1);
    glPushMatrix();
    glTranslatef(-231, 248, -989);
    glScalef(0.2, 1.0, 0.1);
    glutSolidCube(100);
    glPopMatrix();

    glColor3f(0.68, 0.1, 0.1);
    glPushMatrix();
    glTranslatef(-344, 248, -989);
    glScalef(0.2, 1.0, 0.1);
    glutSolidCube(100);
    glPopMatrix();

    glColor3f(0.68, 0.1, 0.1);
    glPushMatrix();
   glTranslatef(-457, 248, -989);
    glScalef(0.2, 1.0, 0.1);
    glutSolidCube(100);
    glPopMatrix();
}

void pagar_lantai1kiri() {
    //CAGAK KIRI
    glColor3f(0.68, 0.1, 0.1);
    glPushMatrix();
    glTranslatef(-463, 248, -867);
    glScalef(0.2, 1.0, 0.1);
    glutSolidCube(100);
    glPopMatrix();

    glColor3f(0.68, 0.1, 0.1);
    glPushMatrix();
    glTranslatef(-463, 248, -747);
    glScalef(0.2, 1.0, 0.1);
    glutSolidCube(100);
    glPopMatrix();

    glColor3f(0.68, 0.1, 0.1);
    glPushMatrix();
    glTranslatef(-463, 248, -632);
    glScalef(0.2, 1.0, 0.1);
    glutSolidCube(100);
    glPopMatrix();

    glColor3f(0.68, 0.1, 0.1);
    glPushMatrix();
    glTranslatef(-463, 248, -496);
    glScalef(0.2, 1.0, 0.1);
    glutSolidCube(100);
    glPopMatrix();

    glColor3f(0.68, 0.1, 0.1);
    glPushMatrix();
    glTranslatef(-463, 248, -369);
    glScalef(0.2, 1.0, 0.1);
    glutSolidCube(100);
    glPopMatrix();

    glColor3f(0.68, 0.1, 0.1);
    glPushMatrix();
    glTranslatef(-463, 248, -242);
    glScalef(0.2, 1.0, 0.1);
    glutSolidCube(100);
    glPopMatrix();

    glColor3f(0.68, 0.1, 0.1);
    glPushMatrix();
    glTranslatef(-463, 248, -114);
    glScalef(0.2, 1.0, 0.1);
    glutSolidCube(100);
    glPopMatrix();


    //pagar atas  kiri
    glColor3f(0.68, 0.1, 0.1);
    glPushMatrix();
    glTranslatef(-452, 298, -486);
    glScalef(0.2, 0.10, 10);
    glutSolidCube(100);
    glPopMatrix();

    //pagar bawah 2 kiri
    glColor3f(0.68, 0.1, 0.1);
    glPushMatrix();
    glTranslatef(-452 , 244, -486 );
    glScalef(0.2, 0.10, 10);
    glutSolidCube(100);
    glPopMatrix();
}

void pagar_lantai1kanan() {
    //CAGAK Kanan
    glColor3f(0.68, 0.1, 0.1);
    glPushMatrix();
    glTranslatef(463, 248, -867);
    glScalef(0.2, 1.0, 0.1);
    glutSolidCube(100);
    glPopMatrix();

    glColor3f(0.68, 0.1, 0.1);
    glPushMatrix();
    glTranslatef(463, 248, -747);
    glScalef(0.2, 1.0, 0.1);
    glutSolidCube(100);
    glPopMatrix();

    glColor3f(0.68, 0.1, 0.1);
    glPushMatrix();
    glTranslatef(463, 248, -632);
    glScalef(0.2, 1.0, 0.1);
    glutSolidCube(100);
    glPopMatrix();

    glColor3f(0.68, 0.1, 0.1);
    glPushMatrix();
    glTranslatef(463, 248, -496);
    glScalef(0.2, 1.0, 0.1);
    glutSolidCube(100);
    glPopMatrix();

    glColor3f(0.68, 0.1, 0.1);
    glPushMatrix();
    glTranslatef(463, 248, -369);
    glScalef(0.2, 1.0, 0.1);
    glutSolidCube(100);
    glPopMatrix();

    glColor3f(0.68, 0.1, 0.1);
    glPushMatrix();
    glTranslatef(463, 248, -242);
    glScalef(0.2, 1.0, 0.1);
    glutSolidCube(100);
    glPopMatrix();

    glColor3f(0.68, 0.1, 0.1);
    glPushMatrix();
    glTranslatef(463, 248, -114);
    glScalef(0.2, 1.0, 0.1);
    glutSolidCube(100);
    glPopMatrix();


    //pagar atas  kiri
    glColor3f(0.68, 0.1, 0.1);
    glPushMatrix();
    glTranslatef(452, 298, -486);
    glScalef(0.2, 0.10, 10);
    glutSolidCube(100);
    glPopMatrix();

    //pagar bawah 2 kiri
    glColor3f(0.68, 0.1, 0.1);
    glPushMatrix();
    glTranslatef(452, 244, -486);
    glScalef(0.2, 0.10, 10);
    glutSolidCube(100);
    glPopMatrix();

}

void pagar_lantai2kanan() {
    //CAGAK Kanan
    glColor3f(0.68, 0.1, 0.1);
    glPushMatrix();
    glTranslatef(463, 248, -867);
    glScalef(0.2, 1.0, 0.1);
    glutSolidCube(100);
    glPopMatrix();

    glColor3f(0.68, 0.1, 0.1);
    glPushMatrix();
    glTranslatef(463, 248, -747);
    glScalef(0.2, 1.0, 0.1);
    glutSolidCube(100);
    glPopMatrix();

    glColor3f(0.68, 0.1, 0.1);
    glPushMatrix();
    glTranslatef(463, 248, -632);
    glScalef(0.2, 1.0, 0.1);
    glutSolidCube(100);
    glPopMatrix();

    glColor3f(0.68, 0.1, 0.1);
    glPushMatrix();
    glTranslatef(463, 248, -496);
    glScalef(0.2, 1.0, 0.1);
    glutSolidCube(100);
    glPopMatrix();

    glColor3f(0.68, 0.1, 0.1);
    glPushMatrix();
    glTranslatef(463, 248, -369);
    glScalef(0.2, 1.0, 0.1);
    glutSolidCube(100);
    glPopMatrix();

    glColor3f(0.68, 0.1, 0.1);
    glPushMatrix();
    glTranslatef(463, 248, -242);
    glScalef(0.2, 1.0, 0.1);
    glutSolidCube(100);
    glPopMatrix();

    glColor3f(0.68, 0.1, 0.1);
    glPushMatrix();
    glTranslatef(463, 248, -114);
    glScalef(0.2, 1.0, 0.1);
    glutSolidCube(100);
    glPopMatrix();


    //pagar atas  kiri
    glColor3f(0.68, 0.1, 0.1);
    glPushMatrix();
    glTranslatef(452, 298, -486);
    glScalef(0.2, 0.10, 10);
    glutSolidCube(100);
    glPopMatrix();

    //pagar bawah 2 kiri
    glColor3f(0.68, 0.1, 0.1);
    glPushMatrix();
    glTranslatef(452, 244, -486);
    glScalef(0.2, 0.10, 10);
    glutSolidCube(100);
    glPopMatrix();

}

void pagar_lantai2kiri() {
    //CAGAK KIRI
    glColor3f(0.68, 0.1, 0.1);
    glPushMatrix();
    glTranslatef(-463, 248, -867);
    glScalef(0.2, 1.0, 0.1);
    glutSolidCube(100);
    glPopMatrix();

    glColor3f(0.68, 0.1, 0.1);
    glPushMatrix();
    glTranslatef(-463, 248, -747);
    glScalef(0.2, 1.0, 0.1);
    glutSolidCube(100);
    glPopMatrix();

    glColor3f(0.68, 0.1, 0.1);
    glPushMatrix();
    glTranslatef(-463, 248, -632);
    glScalef(0.2, 1.0, 0.1);
    glutSolidCube(100);
    glPopMatrix();

    glColor3f(0.68, 0.1, 0.1);
    glPushMatrix();
    glTranslatef(-463, 248, -496);
    glScalef(0.2, 1.0, 0.1);
    glutSolidCube(100);
    glPopMatrix();

    glColor3f(0.68, 0.1, 0.1);
    glPushMatrix();
    glTranslatef(-463, 248, -369);
    glScalef(0.2, 1.0, 0.1);
    glutSolidCube(100);
    glPopMatrix();

    glColor3f(0.68, 0.1, 0.1);
    glPushMatrix();
    glTranslatef(-463, 248, -242);
    glScalef(0.2, 1.0, 0.1);
    glutSolidCube(100);
    glPopMatrix();

    glColor3f(0.68, 0.1, 0.1);
    glPushMatrix();
    glTranslatef(-463, 248, -114);
    glScalef(0.2, 1.0, 0.1);
    glutSolidCube(100);
    glPopMatrix();


    //pagar atas  kiri
    glColor3f(0.68, 0.1, 0.1);
    glPushMatrix();
    glTranslatef(-452, 298, -486);
    glScalef(0.2, 0.10, 10);
    glutSolidCube(100);
    glPopMatrix();

    //pagar bawah 2 kiri
    glColor3f(0.68, 0.1, 0.1);
    glPushMatrix();
    glTranslatef(-452, 244, -486);
    glScalef(0.2, 0.10, 10);
    glutSolidCube(100);
    glPopMatrix();
}

void pagar_lantai2belakang() {
    //DEPAN
    //pagar atas 1 kanan
    glColor3f(0.68, 0.1, 0.1);
    glPushMatrix();
    glTranslatef(283, 297, -989);
    glScalef(3.5, 0.10, 0.2);
    glutSolidCube(100);
    glPopMatrix();


    //pagar bawah 1 kanan
    glColor3f(0.68, 0.1, 0.1);
    glPushMatrix();
    glTranslatef(283, 245, -989);
    glScalef(3.5, 0.10, 0.2);
    glutSolidCube(100);
    glPopMatrix();

    //pagar atas 2 kiri
    glColor3f(0.68, 0.1, 0.1);
    glPushMatrix();
    glTranslatef(-283, 297, -989);
    glScalef(3.5, 0.10, 0.2);
    glutSolidCube(100);
    glPopMatrix();

    //pagar bawah 2 kiri
    glColor3f(0.68, 0.1, 0.1);
    glPushMatrix();
    glTranslatef(-283, 245, -989);
    glScalef(3.5, 0.10, 0.2);
    glutSolidCube(100);
    glPopMatrix();

    //CAGAK
    //CAGAK merah kanan  Depan
    glColor3f(0.68, 0.1, 0.1);
    glPushMatrix();
    glTranslatef(113, 248, -989);
    glScalef(0.2, 1.0, 0.1);
    glutSolidCube(100);
    glPopMatrix();

    glColor3f(0.68, 0.1, 0.1);
    glPushMatrix();
    glTranslatef(231, 248, -989);
    glScalef(0.2, 1.0, 0.1);
    glutSolidCube(100);
    glPopMatrix();

    glColor3f(0.68, 0.1, 0.1);
    glPushMatrix();
    glTranslatef(344, 248, -989);
    glScalef(0.2, 1.0, 0.1);
    glutSolidCube(100);
    glPopMatrix();

    glColor3f(0.68, 0.1, 0.1);
    glPushMatrix();
    glTranslatef(457, 248, -989);
    glScalef(0.2, 1.0, 0.1);
    glutSolidCube(100);
    glPopMatrix();

    //CAGAK merah kiri Depan
    glColor3f(0.68, 0.1, 0.1);
    glPushMatrix();
    glTranslatef(-113, 248, -989);
    glScalef(0.2, 1.0, 0.1);
    glutSolidCube(100);
    glPopMatrix();

    glColor3f(0.68, 0.1, 0.1);
    glPushMatrix();
    glTranslatef(-231, 248, -989);
    glScalef(0.2, 1.0, 0.1);
    glutSolidCube(100);
    glPopMatrix();

    glColor3f(0.68, 0.1, 0.1);
    glPushMatrix();
    glTranslatef(-344, 248, -989);
    glScalef(0.2, 1.0, 0.1);
    glutSolidCube(100);
    glPopMatrix();

    glColor3f(0.68, 0.1, 0.1);
    glPushMatrix();
    glTranslatef(-457, 248, -989);
    glScalef(0.2, 1.0, 0.1);
    glutSolidCube(100);
    glPopMatrix();
}

void pagar_lantai2depan() {
    //DEPAN
    //pagar atas 1 kanan
    glColor3f(0.68, 0.1, 0.1);
    glPushMatrix();
    glTranslatef(283, 297, 4);
    glScalef(3.5, 0.10, 0.2);
    glutSolidCube(100);
    glPopMatrix();

    //pagar bawah 1 kanan
    glColor3f(0.68, 0.1, 0.1);
    glPushMatrix();
    glTranslatef(283, 245, 4);
    glScalef(3.5, 0.10, 0.2);
    glutSolidCube(100);
    glPopMatrix();

    //pagar atas 2 kiri
    glColor3f(0.68, 0.1, 0.1);
    glPushMatrix();
    glTranslatef(-283, 297, 4);
    glScalef(3.5, 0.10, 0.2);
    glutSolidCube(100);
    glPopMatrix();

    //pagar bawah 2 kiri
    glColor3f(0.68, 0.1, 0.1);
    glPushMatrix();
    glTranslatef(-283, 245, 4);
    glScalef(3.5, 0.10, 0.2);
    glutSolidCube(100);
    glPopMatrix();

    //CAGAK
    //CAGAK merah kanan  Depan
    glColor3f(0.68, 0.1, 0.1);
    glPushMatrix();
    glTranslatef(113, 248, -5);
    glScalef(0.2, 1.0, 0.1);
    glutSolidCube(100);
    glPopMatrix();

    glColor3f(0.68, 0.1, 0.1);
    glPushMatrix();
    glTranslatef(231, 248, -5);
    glScalef(0.2, 1.0, 0.1);
    glutSolidCube(100);
    glPopMatrix();

    glColor3f(0.68, 0.1, 0.1);
    glPushMatrix();
    glTranslatef(344, 248, -5);
    glScalef(0.2, 1.0, 0.1);
    glutSolidCube(100);
    glPopMatrix();

    glColor3f(0.68, 0.1, 0.1);
    glPushMatrix();
    glTranslatef(457, 248, -5);
    glScalef(0.2, 1.0, 0.1);
    glutSolidCube(100);
    glPopMatrix();

    //CAGAK merah kiri Depan
    glColor3f(0.68, 0.1, 0.1);
    glPushMatrix();
    glTranslatef(-457, 248, 5);
    glScalef(0.2, 1.0, 0.1);
    glutSolidCube(100);
    glPopMatrix();

    glColor3f(0.68, 0.1, 0.1);
    glPushMatrix();
    glTranslatef(-344, 248, 5);
    glScalef(0.2, 1.0, 0.1);
    glutSolidCube(100);
    glPopMatrix();

    glColor3f(0.68, 0.1, 0.1);
    glPushMatrix();
    glTranslatef(-231, 248, 5);
    glScalef(0.2, 1.0, 0.1);
    glutSolidCube(100);
    glPopMatrix();

    glColor3f(0.68, 0.1, 0.1);
    glPushMatrix();
    glTranslatef(-113, 248, 5);
    glScalef(0.2, 1.0, 0.1);
    glutSolidCube(100);
    glPopMatrix();
}


void dasar1() {
    //layer lantai merah
    glColor3f(1.0, 1.0, 1.0);
    glPushMatrix();
    glTranslatef(0, 793, -542);
    glScalef(1.50, 0.01, 2.00);
    glutSolidCube(500);
    glPopMatrix();

    glColor3f(0.0, 0.0, 0.0);
    glPushMatrix();
    glTranslatef(0, 784, -542);
    glScalef(1.70, 0.01, 2.00);
    glutSolidCube(500);
    glPopMatrix();
}

void pagar_lantai3depan() {
    //DEPAN
    //pagar atas 1 kanan
    glColor3f(0.68, 0.1, 0.1);
    glPushMatrix();
    glTranslatef(257, 283, 4);
    glScalef(3.4, 0.07, 0.2);
    glutSolidCube(100);
    glPopMatrix();

    //pagar bawah 1 kanan
    glColor3f(0.68, 0.1, 0.1);
    glPushMatrix();
    glTranslatef(257, 245, 4);
    glScalef(3.4, 0.07, 0.2);
    glutSolidCube(100);
    glPopMatrix();

    //pagar atas 2 kiri
    glColor3f(0.68, 0.1, 0.1);
    glPushMatrix();
    glTranslatef(-257, 283, 4);
    glScalef(3.4, 0.07, 0.2);
    glutSolidCube(100);
    glPopMatrix();

    //pagar bawah 2 kiri
    glColor3f(0.68, 0.1, 0.1);
    glPushMatrix();
    glTranslatef(-257, 245, 4);
    glScalef(3.5, 0.10, 0.2);
    glutSolidCube(100);
    glPopMatrix();

    //CAGAK
    //CAGAK merah kanan  Depan
    glColor3f(0.68, 0.1, 0.1);
    glPushMatrix();
    glTranslatef(89, 248, -5);
    glScalef(0.1, 0.7, 0.1);
    glutSolidCube(100);
    glPopMatrix();

    glColor3f(0.68, 0.1, 0.1);
    glPushMatrix();
    glTranslatef(200, 248, -5);
    glScalef(0.1, 0.7, 0.1);
    glutSolidCube(100);
    glPopMatrix();

    glColor3f(0.68, 0.1, 0.1);
    glPushMatrix();
    glTranslatef(329, 248, -5);
    glScalef(0.1, 0.7, 0.1);
    glutSolidCube(100);
    glPopMatrix();

    glColor3f(0.68, 0.1, 0.1);
    glPushMatrix();
    glTranslatef(421, 240, -5);
    glScalef(0.1, 0.8, 0.1);
    glutSolidCube(100);
    glPopMatrix();

    //CAGAK merah kiri Depan
    glColor3f(0.68, 0.1, 0.1);
    glPushMatrix();
    glTranslatef(-89, 248, -5);
    glScalef(0.1, 0.7, 0.1);
    glutSolidCube(100);
    glPopMatrix();

    glColor3f(0.68, 0.1, 0.1);
    glPushMatrix();
    glTranslatef(-200, 248, -5);
    glScalef(0.1, 0.7, 0.1);
    glutSolidCube(100);
    glPopMatrix();

    glColor3f(0.68, 0.1, 0.1);
    glPushMatrix();
    glTranslatef(-329, 248, -5);
    glScalef(0.1, 0.7, 0.1);
    glutSolidCube(100);
    glPopMatrix();

    glColor3f(0.68, 0.1, 0.1);
    glPushMatrix();
    glTranslatef(-421, 240, -5);
    glScalef(0.1, 0.8, 0.1);
    glutSolidCube(100);
    glPopMatrix();
}

void pagar_lantai3kiri() {
    //CAGAK KIRI
    glColor3f(0.68, 0.1, 0.1);
    glPushMatrix();
    glTranslatef(-463, 248, -867);
    glScalef(0.1, 0.84, 0.1);
    glutSolidCube(100);
    glPopMatrix();

    glColor3f(0.68, 0.1, 0.1);
    glPushMatrix();
    glTranslatef(-463, 248, -747);
    glScalef(0.1, 0.84, 0.1);
    glutSolidCube(100);
    glPopMatrix();

    glColor3f(0.68, 0.1, 0.1);
    glPushMatrix();
    glTranslatef(-463, 248, -632);
    glScalef(0.1, 0.84, 0.1);
    glutSolidCube(100);
    glPopMatrix();

    glColor3f(0.68, 0.1, 0.1);
    glPushMatrix();
    glTranslatef(-463, 248, -496);
    glScalef(0.1, 0.84, 0.1);
    glutSolidCube(100);
    glPopMatrix();

    glColor3f(0.68, 0.1, 0.1);
    glPushMatrix();
    glTranslatef(-463, 248, -369);
    glScalef(0.1, 0.84, 0.1);
    glutSolidCube(100);
    glPopMatrix();

    glColor3f(0.68, 0.1, 0.1);
    glPushMatrix();
    glTranslatef(-463, 248, -242);
    glScalef(0.1, 0.84, 0.1);
    glutSolidCube(100);
    glPopMatrix();

    glColor3f(0.68, 0.1, 0.1);
    glPushMatrix();
    glTranslatef(-463, 248, -114);
    glScalef(0.1, 0.84, 0.1);
    glutSolidCube(100);
    glPopMatrix();

    //pagar atas  kiri
    glColor3f(0.68, 0.1, 0.1);
    glPushMatrix();
    glTranslatef(-452, 283, -486);
    glScalef(0.2, 0.07, 10);
    glutSolidCube(100);
    glPopMatrix();

    //pagar bawah 2 kiri
    glColor3f(0.68, 0.1, 0.1);
    glPushMatrix();
    glTranslatef(-452, 245, -486);
    glScalef(0.2, 0.07, 10);
    glutSolidCube(100);
    glPopMatrix();
}

void pagar_lantai3belakang() {
    //DEPAN
    //pagar atas 1 kanan
    glColor3f(0.68, 0.1, 0.1);
    glPushMatrix();
    glTranslatef(283, 297, -989);
    glScalef(3.5, 0.10, 0.2);
    glutSolidCube(100);
    glPopMatrix();

    //pagar bawah 1 kanan
    glColor3f(0.68, 0.1, 0.1);
    glPushMatrix();
    glTranslatef(283, 245, -989);
    glScalef(3.5, 0.10, 0.2);
    glutSolidCube(100);
    glPopMatrix();

    //pagar atas 2 kiri
    glColor3f(0.68, 0.1, 0.1);
    glPushMatrix();
    glTranslatef(-283, 297, -989);
    glScalef(3.5, 0.10, 0.2);
    glutSolidCube(100);
    glPopMatrix();

    //pagar bawah 2 kiri
    glColor3f(0.68, 0.1, 0.1);
    glPushMatrix();
    glTranslatef(-283, 245, -989);
    glScalef(3.5, 0.10, 0.2);
    glutSolidCube(100);
    glPopMatrix();

    //CAGAK
    //CAGAK merah kanan  Depan
    glColor3f(0.68, 0.1, 0.1);
    glPushMatrix();
    glTranslatef(113, 248, -989);
    glScalef(0.1, 0.84, 0.1);
    glutSolidCube(100);
    glPopMatrix();

    glColor3f(0.68, 0.1, 0.1);
    glPushMatrix();
    glTranslatef(231, 248, -989);
    glScalef(0.1, 0.84, 0.1);
    glutSolidCube(100);
    glPopMatrix();

    glColor3f(0.68, 0.1, 0.1);
    glPushMatrix();
    glTranslatef(344, 248, -989);
    glScalef(0.1, 0.84, 0.1);
    glutSolidCube(100);
    glPopMatrix();

    glColor3f(0.68, 0.1, 0.1);
    glPushMatrix();
    glTranslatef(457, 248, -989);
    glScalef(0.1, 0.84, 0.1);
    glutSolidCube(100);
    glPopMatrix();

    //CAGAK merah kiri Depan
    glColor3f(0.68, 0.1, 0.1);
    glPushMatrix();
    glTranslatef(-113, 248, -989);
    glScalef(0.1, 0.84, 0.1);
    glutSolidCube(100);
    glPopMatrix();

    glColor3f(0.68, 0.1, 0.1);
    glPushMatrix();
    glTranslatef(-231, 248, -989);
    glScalef(0.1, 0.84, 0.1);
    glutSolidCube(100);
    glPopMatrix();

    glColor3f(0.68, 0.1, 0.1);
    glPushMatrix();
    glTranslatef(-344, 248, -989);
    glScalef(0.1, 0.84, 0.1);
    glutSolidCube(100);
    glPopMatrix();

    glColor3f(0.68, 0.1, 0.1);
    glPushMatrix();
    glTranslatef(-457, 248, -989);
    glScalef(0.1, 0.84, 0.1);
    glutSolidCube(100);
    glPopMatrix();
}

void pagar_lantai3kanan() {
    //CAGAK KIRI
    glColor3f(0.68, 0.1, 0.1);
    glPushMatrix();
    glTranslatef(463, 248, -867);
    glScalef(0.1, 0.84, 0.1);
    glutSolidCube(100);
    glPopMatrix();

    glColor3f(0.68, 0.1, 0.1);
    glPushMatrix();
    glTranslatef(463, 248, -747);
    glScalef(0.1, 0.84, 0.1);
    glutSolidCube(100);
    glPopMatrix();

    glColor3f(0.68, 0.1, 0.1);
    glPushMatrix();
    glTranslatef(463, 248, -632);
    glScalef(0.1, 0.84, 0.1);
    glutSolidCube(100);
    glPopMatrix();

    glColor3f(0.68, 0.1, 0.1);
    glPushMatrix();
    glTranslatef(463, 248, -496);
    glScalef(0.1, 0.84, 0.1);
    glutSolidCube(100);
    glPopMatrix();

    glColor3f(0.68, 0.1, 0.1);
    glPushMatrix();
    glTranslatef(463, 248, -369);
    glScalef(0.1, 0.84, 0.1);
    glutSolidCube(100);
    glPopMatrix();

    glColor3f(0.68, 0.1, 0.1);
    glPushMatrix();
    glTranslatef(463, 248, -242);
    glScalef(0.1, 0.84, 0.1);
    glutSolidCube(100);
    glPopMatrix();

    glColor3f(0.68, 0.1, 0.1);
    glPushMatrix();
    glTranslatef(463, 248, -114);
    glScalef(0.1, 0.84, 0.1);
    glutSolidCube(100);
    glPopMatrix();

    //pagar atas  kiri
    glColor3f(0.68, 0.1, 0.1);
    glPushMatrix();
    glTranslatef(452, 283, -486);
    glScalef(0.2, 0.07, 10);
    glutSolidCube(100);
    glPopMatrix();

    //pagar bawah 2 kiri
    glColor3f(0.68, 0.1, 0.1);
    glPushMatrix();
    glTranslatef(452, 245, -486);
    glScalef(0.2, 0.07, 10);
    glutSolidCube(100);
    glPopMatrix();
}

void dasar2() {
    //layer lantai merah
    glColor3f(1.0, 1.0, 1.0);
    glPushMatrix();
    glTranslatef(0, 948, -542);
    glScalef(1.0, 0.01, 2.00);
    glutSolidCube(500);
    glPopMatrix();

    glColor3f(0.0, 0.0, 0.0);
    glPushMatrix();
    glTranslatef(0, 945, -542);
    glScalef(1.30, 0.01, 2.00);
    glutSolidCube(500);
    glPopMatrix();
}




void draw() {
    // Mulai tuliskan isi pikiranmu disini
    glutWireCube(1000.0);
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

    pagar_lantai1depan();
    pagar_lantai1belakang();
    pagar_lantai1kiri();
    pagar_lantai1kanan();
    dasar1();

    glPushMatrix();
    glTranslatef(0, 318, 0);
    pagar_lantai2kanan();
    glPopMatrix();

    glPushMatrix();
    glTranslatef(0, 318 , 0);
    pagar_lantai2kiri();
    glPopMatrix();

    glPushMatrix();
    glTranslatef(0, 318 , 0);
    pagar_lantai2belakang();
    glPopMatrix();

    glPushMatrix();
    glTranslatef(0, 318 , -27);
    pagar_lantai2depan();
    glPopMatrix();


    glPushMatrix();
    glTranslatef(0, 584, -82);
    pagar_lantai3depan();
    glPopMatrix();

    glPushMatrix();
    glTranslatef(42, 584, -82);
    pagar_lantai3kiri();
    glPopMatrix();

    glPushMatrix();
    glTranslatef(42, 584, -82);
    pagar_lantai3belakang();
    glPopMatrix();


    glPushMatrix();
    glTranslatef(-30 , 584 , -82 );
    pagar_lantai3kanan();
    glPopMatrix();

    dasar2();

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
