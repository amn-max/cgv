#include <stdio.h>
#include <GL/glut.h>

int x, y;
int where_to_rotate;
float rotate_angle = 0;
float translate_x = 0, translate_y = 0;

void init() {
    glClearColor(0, 0, 0, 1);
    glMatrixMode(GL_PROJECTION);
    glLoadIdentity();
    gluOrtho2D(-800, 800, -800, 800);
    glMatrixMode(GL_MODELVIEW);
}

void drawPoint(int x, int y) {
    glPointSize(5);
    glBegin(GL_POINTS);
    glVertex2f(x, y);
    glEnd();
}


void triangle(int x, int y) {
    glColor3f(1, 0, 0);
    glBegin(GL_POLYGON);
    glVertex2f(x, y);
    glVertex2f(x + 400, y + 300);
    glVertex2f(x + 300, y + 0);
    glEnd();
}

void display() {
    glClear(GL_COLOR_BUFFER_BIT);
    glLoadIdentity();

    glColor3f(1, 1, 1);
    drawPoint(0, 0);

    if (where_to_rotate == 1) {
        translate_x = 0;
        translate_y = 0;
        rotate_angle += 1;
    }

    if (where_to_rotate == 2) {
        translate_x = x;
        translate_y = y;
        rotate_angle += 1;
        glColor3f(0, 0, 1);
        drawPoint(x, y);
    }
    glTranslatef(translate_x, translate_y, 0);
    glRotatef(rotate_angle, 0, 0, 1);
    glTranslatef(-translate_x, -translate_y, 0);

    triangle(translate_x, translate_y);

    glutPostRedisplay();
    glutSwapBuffers();
    glFlush();
}

void menu(int option)
{
    if (option == 1)
        where_to_rotate = 1;      

    if (option == 2)
        where_to_rotate = 2;      

    if (option == 3)
        where_to_rotate = 3;     
}

int main(int argc, char** argv) {
    
    printf("Enter Fixed Points (x,y) for Rotation: \n");
    scanf_s("%d%d", &x, &y);

    glutInit(&argc, argv);
    glutInitDisplayMode(GLUT_DOUBLE | GLUT_RGB);
    glutInitWindowSize(800, 800);
    glutInitWindowPosition(200, 200);
    glutCreateWindow("Rotate Triangle");
    init();
    glutDisplayFunc(display);

    glutCreateMenu(menu);
    glutAddMenuEntry("Rotate around ORIGIN", 1);
    glutAddMenuEntry("Rotate around FIXED POINT", 2);
    glutAddMenuEntry("Stop Rotation", 3);
    glutAttachMenu(GLUT_RIGHT_BUTTON);
    glutMainLoop();
}
