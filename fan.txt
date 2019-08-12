#include <windows.h>
#include <GL/glut.h>
#include <stdio.h>
#include <GL/glu.h>
#include <GL/GLU.h>
#include <stdlib.h>
#include <string.h>
//#include <unistd.h>
/* Global variables */
bool rotate = false;
double holder_ht = 0.1;
GLfloat gAngle = 0.0;
double base_angle = 0.0;
double cube_angle = 0.0;
/* Initialize OpenGL Graphics */

void timer(int value)
{
	const int desiredFPS = 120;
	glutTimerFunc(1000 / desiredFPS, timer, ++value);
	GLfloat dt = 1. / desiredFPS;


	gAngle += dt * 360. / 1;
	glutPostRedisplay();
}


void initGL() {
	glClearColor(0.0f, 0.0f, 0.5f, 1.0f); // Set background color to black and opaque
	glClearDepth(1.0f);                   // Set background depth to farthest
	glEnable(GL_DEPTH_TEST);   // Enable depth testing for z-culling
	glDepthFunc(GL_LEQUAL);    // Set the type of depth-test
	glShadeModel(GL_SMOOTH);   // Enable smooth shading
	glHint(GL_PERSPECTIVE_CORRECTION_HINT, GL_NICEST);
}

void drawFan() {
	//glPushMatrix();
	glRotatef(cube_angle, 0.0, 1.0, 0.0);

	glPushMatrix();
	glRotatef(base_angle, 1.0, -.5, -.5);


	glTranslatef(0, -holder_ht * 3, -0.5);

	glTranslatef(-1.2, 0.0475, 0);
	glRotatef(60, 0.5, -0.7, 0);

	for (int i = 0; i < 3; i++)
	{

		glPushMatrix();
		glPushMatrix();

		glTranslatef(0, 3 * holder_ht, 0);
		glRotatef(90., 1.0, 0.0, 0.0);
		glScalef(1.0, 0, 2.0);
		glColor3f(0.0f, 0.0f, 1.0f);
		glutSolidCube(holder_ht / 2); //holding stick.
		glPopMatrix();
		glTranslatef(0, holder_ht, 0);
		if (rotate) {
			glRotatef(gAngle, 0., 1., 0.);
		}
		glRotatef(120 * i, 0.0, 1.0, 0.0);
		glPushMatrix();
		glTranslatef(0.05, holder_ht + 0.0275, 0);
		glScalef(0.5, 0.1, 2.0);
		glColor3f(0.8, 1.0, 0.0);
		glutSolidCube(0.05);
		glPopMatrix();

		glPushMatrix();
		glTranslatef(-0.05, holder_ht + 0.0275, 0);
		glScalef(0.5, 0.1, 2.0);
		glColor3f(0.8, 1.0, 0.0);
		glutSolidCube(0.05);
		glPopMatrix();

		glTranslatef(0, holder_ht + 0.0275, 0.340);
		glScalef(1.0, 0.1, 8.0);
		glColor3f(1.0, 0.0, 0.0);
		glutSolidCube(0.05); //blade
		glPopMatrix();
	}



	glPopMatrix();

}
void reshape(GLsizei width, GLsizei height) {		//called when window is resized
	GLfloat aspect = (GLfloat)width / (GLfloat)height;
	glViewport(0.0, 0, width, height);		// to set y view port
	glMatrixMode(GL_PROJECTION);
	glLoadIdentity();
	gluPerspective(45.0f, aspect, 0.1f, 100.0f);

}
void keyboard(unsigned char key, int x, int y)
{
	int countt = 0;int counttt = 5;
	switch (key) {
	case 'q':
		exit(0);
		break;

	case 's':
		rotate = true;
		break;
	case 't': rotate = false;
		break;
	case 'u':
		if (holder_ht > 0.05)holder_ht -= 0.005;
		break;
	case 'd':
		if (holder_ht < 0.15)holder_ht += 0.005;
		break;
	case 'h':
		//lateral=true;
		while (counttt > 0) {
			while (countt < 3) {
				printf("Left");
				keyboard('r', x, y);
				countt++;
				//  glutReshapeFunc(reshape);
				Sleep(1000);
			}
			//Sleep(1);
			//Sleep(1);

			Sleep(5000);
			while (countt > -30) {
				printf("Right");countt--;
				keyboard('l', x, y);
				//	glutReshapeFunc(reshape);
				Sleep(100);
			}
			Sleep(1);
			counttt--;
		}
		break;
	case 'l':
		if (base_angle < 30) base_angle++;
		break;
	case 'r':
		if (base_angle > -30)base_angle--;
		break;

	case 'a': cube_angle = 0;
		break;

	case 'b': cube_angle = 10;
		break;

	case 'c': cube_angle = -20;
		break;

	default:
		break;
	}

}

void room(void);
void display() {
	glClear(GL_COLOR_BUFFER_BIT | GL_DEPTH_BUFFER_BIT);
	glFlush();
	glMatrixMode(GL_MODELVIEW);
	glLoadIdentity();
	glTranslatef(0.f, 0.5f, -2.0f);
	room();
}

void room() {
	glClear(GL_COLOR_BUFFER_BIT | GL_DEPTH_BUFFER_BIT);

	drawFan();
	glPushMatrix();
	glTranslatef(0, -0.7, 0);
	glScalef(0.1, 0.1, 0.1);
	glBegin(GL_QUADS);           // Begin drawing the cubes.
	   // Front face
	glColor3f(0.0f, 1.0f, 0.0f);
	glVertex3f(1.5f, 1.0f, -2.0f);
	glVertex3f(-1.5f, 1.0f, -2.0f);
	glColor3f(0.0f, 0.50f, .0f);
	glVertex3f(-1.5f, 1.0f, 2.0f);
	glVertex3f(1.5f, 1.0f, 2.0f);

	// Bottom face (y = -1.0f)
	glColor3f(0.8f, 0.8f, 0.8f);
	glVertex3f(1.5f, -1.0f, 2.0f);
	glVertex3f(-1.5f, -1.0f, 2.0f);
	glColor3f(0.8f, 0.8f, 0.8f);
	glVertex3f(-1.5f, -1.0f, -2.0f);
	glVertex3f(1.5f, -1.0f, -2.0f);

	// Back face
	glColor3f(.0f, .0f, 0.0f);
	glVertex3f(1.5f, -1.0f, -2.0f);
	glColor3f(.0f, .0f, 01.0f);
	glVertex3f(-1.5f, -1.0f, -2.0f);
	glColor3f(1.0f, .0f, 0.0f);
	glVertex3f(-1.5f, 1.0f, -2.0f);
	glColor3f(0.0f, 1.0f, 0.0f);
	glVertex3f(1.5f, 1.0f, -2.0f);


	// Left face
	glColor3f(1.0f, 0.0f, 1.0f);
	glVertex3f(-1.5f, 1.0f, 2.0f);
	glVertex3f(-1.5f, 1.0f, -2.0f);
	glColor3f(1.0f, 0.0f, 0.5f);
	glVertex3f(-1.5f, -1.0f, -2.0f);
	glVertex3f(-1.5f, -1.0f, 2.0f);

	// Right face
	glColor3f(1.0f, 0.0f, 1.0f);
	glVertex3f(1.5f, 1.0f, -2.0f);
	glVertex3f(1.5f, 1.0f, 2.0f);
	glColor3f(0.7f, 0.0f, 0.7f);
	glVertex3f(1.5f, -1.0f, 2.0f);
	glVertex3f(1.5f, -1.0f, -2.0f);
	glEnd();
	glPopMatrix();
	glBegin(GL_QUADS);
	glColor3f(0.0f, 1.0f, 0.0f);
	glVertex3f(1.5f, 1.0f, -2.0f);
	glVertex3f(-1.5f, 1.0f, -2.0f);
	glColor3f(0.0f, 0.50f, .0f);
	glVertex3f(-1.5f, 1.0f, 2.0f);
	glVertex3f(1.5f, 1.0f, 2.0f);

	// Bottom face (y = -1.0f)
	glColor3f(1.0, 1.0, 0);
	glVertex3f(1.5f, -1.0f, 2.0f);
	glVertex3f(-1.5f, -1.0f, 2.0f);
	glColor3f(1.0, 1.0, 0);
	glVertex3f(-1.5f, -1.0f, -2.0f);
	glVertex3f(1.5f, -1.0f, -2.0f);

	// Back face
	glColor3f(.0f, .0f, 0.0f);
	glVertex3f(1.5f, -1.0f, -2.0f);
	glColor3f(.0f, .0f, 01.0f);
	glVertex3f(-1.5f, -1.0f, -2.0f);
	glColor3f(1.0f, .0f, 0.0f);
	glVertex3f(-1.5f, 1.0f, -2.0f);
	glColor3f(0.0f, 1.0f, 0.0f);
	glVertex3f(1.5f, 1.0f, -2.0f);


	// Left face
	//glColor3f(0.0f, 0.0f, 1.0f);
	glColor3f(1.0f, 0.0f, 1.0f);
	glVertex3f(-1.5f, 1.0f, 2.0f);
	glVertex3f(-1.5f, 1.0f, -2.0f);
	//glColor3f(0.0f, 0.0f, 0.5f);
	glColor3f(0.7f, 0.0f, 0.7f);
	glVertex3f(-1.5f, -1.0f, -2.0f);
	glVertex3f(-1.5f, -1.0f, 2.0f);

	// Right face
	glColor3f(1.0f, 0.0f, 1.0f);
	glVertex3f(1.5f, 1.0f, -2.0f);
	glVertex3f(1.5f, 1.0f, 2.0f);
	glColor3f(0.7f, 0.0f, 0.7f);
	glVertex3f(1.5f, -1.0f, 2.0f);
	glVertex3f(1.5f, -1.0f, -2.0f);

	glEnd();
	glutSwapBuffers();
}




/* Main function: GLUT runs as a console application starting at main() */
int main(int argc, char** argv) {
	glutInit(&argc, argv);            // Initialize GLUT
	glutInitDisplayMode(GLUT_DOUBLE); // Enable double buffered mode
	glutInitWindowSize(1280, 720);   // Set the window's initial width & height
	glutInitWindowPosition(50, 50); // Position the window's initial top-left corner
	glutCreateWindow("Wall Fan");// Create window
	glutTimerFunc(0, timer, 0);
	glutDisplayFunc(display);       // Register callback handler for window re-paint event
	glutReshapeFunc(reshape);       // Register callback handler for window re-size event
	glutKeyboardFunc(keyboard);
	initGL();
	glutMainLoop();                 // Enter the infinite event-processing loop
	return 0;
}
