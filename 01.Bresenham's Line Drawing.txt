#include <stdio.h>
#include <GL/glut.h>

int x1, y1, x2, y2;


void init() {
    glClearColor(1, 1, 1, 1);
    gluOrtho2D(0.0, 500.0, 0.0, 500.0);
}

void drawPoint(int x, int y) {
    glColor3f(0.0, 0.0, 0.0);
    glBegin(GL_POINTS);
    glVertex2i(x, y);
    glEnd();
}

void bresenhamLine(int x1, int y1, int x2, int y2) {
   /* float dx = x2 - x1;
    float dy = x2 - y1;
    
    float dp = 2 * dy - dx;

    int x = x1;
    int y = y1;
    while (x <= x2) {
        drawPoint(x, y);
        x++;
        if (dp >= 0) {
            dp = dp + 2 * dy - 2 * dx;
            y++;
        }
        else if (dp < 0) {
            dp = dp + 2 * dy;
        }
    }*/

    float dx = x2 - x1;
    float dy = y2 - y1;
    float m = dy / dx;

    if (m < 1) {
        int dp = 2 * dy - dx;
        int x = x1;
        int y = y1;
        if (dx < 0) {
            x = x2;
            y = y2;
            x2 = x1;
        }
        drawPoint(x, y);
        while (x < x2) {
            if (dp >= 0) {
                x++;
                y++;
                dp = dp + 2 * dy - 2 * dx * (y + 1 - y);
            }
            else {
                x++;
                y = y;
                dp = dp + 2 * dy - 2 * dx * (y - y);
            }
            drawPoint(x, y);
        }
    }
    else if (m > 1) {
        int dp = 2 * dx - dy;
        int x = x1;
        int y = y1;

        if (dy < 0) {
            x = x2;
            y = y2;
            y2 = y1;
        }
        drawPoint(x, y);
        while (y<y2){
            if (dp >= 0) {
                x++;
                y++;
                dp = dp + 2 * dx - 2 * dy * (x + 1 - x);
            }else {
                y++;
                x = x;
                dp = dp + 2 * dx - 2 * dy * (x - x);
            }
            drawPoint(x, y);
        }
    }
    else if (m == 1) {
        int x = x1;
        int y = y1;
        drawPoint(x, y);
        while (x < x2) {
            x++;
            y++;
            drawPoint(x, y);
        }
    }
}

void display() {
    glClear(GL_COLOR_BUFFER_BIT);
    //line
    bresenhamLine(x1, y1, x2, y2);
    glFlush();
}

int main(int argc, char** argv) {
    
    printf("Enter start Point (x1,y1): ");
    scanf_s("%d%d", &x1, &y1);

    printf("Enter end Point (x2,y2): ");
    scanf_s("%d%d", &x2, &y2);

    glutInit(&argc, argv);
    glutInitDisplayMode(GLUT_SINGLE | GLUT_RGB);
    glutInitWindowSize(500, 500);
    glutInitWindowPosition(200, 200);
    glutCreateWindow("Bresenham's Line");
    init();
    glutDisplayFunc(display);
    glutMainLoop();
}
