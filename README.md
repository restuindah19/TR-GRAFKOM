# TR-GRAFKOM

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

void lantai1() {
    //depan
    glPushMatrix();
    glTranslatef(0, 0, -300);
    glColor3f(1.0f, 1.0f, 1.0f);
    glBegin(GL_POLYGON);
    glVertex3f(-500, 50, -200);
    glVertex3f(-500, 0, -200);
    glVertex3f(500, 0, -200);
    glVertex3f(500, 50, -200);
    glEnd();
    glPopMatrix();

    //belakang
    glPushMatrix();
    glColor3f(1.0f, 1.0f, 1.0f);
    glBegin(GL_POLYGON);
    glVertex3f(-500, 50, 500);
    glVertex3f(-500, 0, 500);
    glVertex3f(500, 0, 500);
    glVertex3f(500, 50, 500);
    glEnd();
    glPopMatrix();

    //kiri
    glPushMatrix();
    glColor3f(1.0f, 1.0f, 1.0f);
    glBegin(GL_POLYGON);
    glVertex3f(-500, 50, -500);
    glVertex3f(-500, 0, -500);
    glVertex3f(-500, 0, 500);
    glVertex3f(-500, 50, 500);
    glEnd();
    glPopMatrix();

    //KANAN
    glPushMatrix();
    glColor3f(1.0f, 1.0f, 1.0f);
    glBegin(GL_POLYGON);
    glVertex3f(500, 50, -500);
    glVertex3f(500, 0, -500);
    glVertex3f(500, 0, 500);
    glVertex3f(500, 50, 500);
    glEnd();
    glPopMatrix();

    //atas
    glPushMatrix();
    glColor3f(1.0f, 1.0f, 1.0f);
    glBegin(GL_POLYGON);
    glVertex3f(-500, 50, 500);
    glVertex3f(-500, 50, -500);
    glVertex3f(500, 50, -500);
    glVertex3f(500, 50, 500);
    glEnd();
    glPopMatrix();

    //lantai2
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
    glColor3f(0.0f, 0.0f, 0.0f);
    glBegin(GL_POLYGON);
    glVertex3f(-600, 300, -500);
    glVertex3f(600, 300, -500);
    glVertex3f(600, 300, 500);
    glVertex3f(-600, 300, 500);
    glEnd();
    glPopMatrix();
}

void pagar_lantai1() {

}

void draw() {
    // Mulai tuliskan isi pikiranmu disini
    glutWireCube(1000.0);
    lantai1();
    //depan
    glPushMatrix();
    glTranslatef(0, 0, 224);
    glColor3f(0.0f, 0.0f, 1.0f);
    glBegin(GL_POLYGON);
    glVertex3f(-350, 550, 100);
    glVertex3f(-350, 300, 100);
    glVertex3f(350, 300, 100);
    glVertex3f(350, 550, 100);
    glEnd();
    glPopMatrix();

    //belakang
    glPushMatrix();
    glColor3f(0.0f, 0.0f, 1.0f);
    glBegin(GL_POLYGON);
    glVertex3f(-350, 550, -400);
    glVertex3f(-350, 300, -400);
    glVertex3f(350, 300, -400);
    glVertex3f(350, 550, -400);
    glEnd();
    glPopMatrix();

    //kiri
    glPushMatrix();
    glColor3f(0.0f, 0.0f, 1.0f);
    glBegin(GL_POLYGON);
    glVertex3f(-350, 550, -400);
    glVertex3f(-350, 300, -400);
    glVertex3f(-350, 300, 330);
    glVertex3f(-350, 550, 330);
    glEnd();
    glPopMatrix();

    //KANAN
    glPushMatrix();
    glColor3f(0.0f, 0.0f, 1.0f);
    glBegin(GL_POLYGON);
    glVertex3f(350, 550, -400);
    glVertex3f(350, 300, -400);
    glVertex3f(350, 300, 330);
    glVertex3f(350, 550, 330);
    glEnd();
    glPopMatrix();

    //atas
    glPushMatrix();
    glColor3f(0.0f, 0.0f, 0.0f);
    glBegin(GL_POLYGON);
    glVertex3f(-350, 550, -400);
    glVertex3f(350, 550, -400);
    glVertex3f(350, 550, 330);
    glVertex3f(-350, 550, 330);
    glEnd();
    glPopMatrix();
    //atap lantai 1
   //atas
    glPushMatrix();
    glTranslatef(0, 250.5, 0);
    glColor3f(0.0f, 1.0f, 1.0f);
    glBegin(GL_POLYGON);
    glVertex3f(-600, 300, -500);
    glVertex3f(600, 300, -500);
    glVertex3f(600, 300, 500);
    glVertex3f(-600, 300, 500);
    glEnd();
    glPopMatrix();
    //KANAN
    glPushMatrix();
    glTranslatef(0, 500, 0);
    glColor3f(0.0f, 1.0f, 0.0f);
    glBegin(GL_POLYGON);
    glVertex3f(600, 50, -500);
    glVertex3f(300, 300, -170);
    glVertex3f(300, 300, 170);
    glVertex3f(600, 50, 500);
    glEnd();
    glPopMatrix();
    //kiri
    glPushMatrix();
    glTranslatef(0, 500, 0);
    glColor3f(1.0f, 0.0f, 1.0f);
    glBegin(GL_POLYGON);
    glVertex3f(-600, 50, -500);
    glVertex3f(-300, 300, -170);
    glVertex3f(-300, 300, 170);
    glVertex3f(-600, 50, 500);
    glEnd();
    glPopMatrix();
    //belakang
    glPushMatrix();
    glTranslatef(0, 500, 0);
    glColor3f(1.0f, 0.0f, 0.0f);
    glBegin(GL_POLYGON);
    glVertex3f(-600, 50, -500);
    glVertex3f(-300, 300, -170);
    glVertex3f(300, 300, -170);
    glVertex3f(600, 50, -500);
    glEnd();
    glPopMatrix();
    //depan
    glPushMatrix();
    glTranslatef(0, 500, 0);
    glColor3f(1.0f, 1.0f, 1.0f);
    glBegin(GL_POLYGON);
    glVertex3f(-600, 50, 500);
    glVertex3f(-300, 300, 170);
    glVertex3f(300, 300, 170);
    glVertex3f(600, 50, 500);
    glEnd();
    glPopMatrix();

    //lantai 3
    //dasar
    glPushMatrix();
    glTranslatef(0,500,0);
    glColor3f(0.0f, 1.0f, 1.0f);
    glBegin(GL_POLYGON);
    glVertex3f(-400, 300, -300);
    glVertex3f(400, 300, -300);
    glVertex3f(400, 300, 300);
    glVertex3f(-400, 300, 300);
    glEnd();
    glPopMatrix();

    //kanan 
    glPushMatrix();
    glTranslatef(0, 501, 0);
    glColor3f(0.0f, 0.0f, 1.0f);
    glBegin(GL_POLYGON);
    glVertex3f(350, 550, -400);
    glVertex3f(350, 300, -400);
    glVertex3f(350, 300, 330);
    glVertex3f(350, 550, 330);
    glEnd();
    glPopMatrix();
    
    //kiri 
    glPushMatrix();
    glTranslatef(0, 501, 0);
    glColor3f(0.0f, 0.0f, 1.0f);
    glBegin(GL_POLYGON);
    glVertex3f(-350, 550, -400);
    glVertex3f(-350, 300, -400);
    glVertex3f(-350, 300, 330);
    glVertex3f(-350, 550, 330);
    glEnd();
    glPopMatrix();

    //depan
    glPushMatrix();
    glTranslatef(0,501, 0);
    glColor3f(0.0f, 0.0f, 1.0f);
    glBegin(GL_POLYGON);
    glVertex3f(-350, 550, 100);
    glVertex3f(-350, 300, 100);
    glVertex3f(350, 300, 100);
    glVertex3f(350, 550, 100);
    glEnd();
    glPopMatrix();


    //belakang
    glPushMatrix();
    glTranslatef(-1.5, 502, 151);
    glColor3f(1.0f, 0.0f, 0.0f);
    glBegin(GL_POLYGON);
    glVertex3f(-350, 550, -400);
    glVertex3f(-350, 300, -400);
    glVertex3f(350, 300, -400);
    glVertex3f(350, 550, -400);
    glEnd();
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
    case '6':
        x_geser += 0.5;
        break;
    case '4':
        x_geser -= 0.5;
        break;
    case '8':
        y_geser += 0.5;
        break;
    case '2':
        y_geser -= 0.5;
        break;
    case '9':
        z_geser -= 0.5;
        break;
    case '1':
        z_geser += 0.5;
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

