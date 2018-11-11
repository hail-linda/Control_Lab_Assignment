import cv2
import numpy as np

THRESHOLD = 40
N = 12


circle_list=[
	[2,2],[-2,2],[2,-2],[-2,-2],
	[3,-1],[3,0],[3,1],[-3,-1],
	[-3,0],[-3,1],[-1,3],[0,3],
	[1,3],[-1,-3],[0,-3],[1,-3]
	]

src = cv2.imread('src.jpg')

gray = np.zeros(src.shape)

for i in range(gray.shape[0]):
	if i%100 == 0:
		print(i)
	for j in range(gray.shape[1]):
		g = sum(src[i,j,0:3])/3
		gray[i,j,0] = g
		gray[i,j,1] = g
		gray[i,j,2] = g

print(gray.shape)

result = np.zeros(src.shape)

for i in range(5,gray.shape[0]-5):
	if i%100 == 0:
		print(i)
	for j in range(5,gray.shape[1]-5):
		s = 0
		for k in range(4):
			a=i+circle_list[k][0]
			b=j+circle_list[k][1]
			if abs(gray[a,b,1]-gray[i,j,1]) > THRESHOLD :
				s += 1
		if s > 2 :
			s = 0
			for k in range(16):
				a=i+circle_list[k][0]
				b=j+circle_list[k][1]
				if abs(gray[a,b,1]-gray[i,j,1]) > THRESHOLD :
					s += 1
			if s > N or s == N :
				result[i,j,0]=0
				result[i,j,1]=0
				result[i,j,2]=255
			else :
				result[i,j,0]=gray[i,j,0]
				result[i,j,1]=gray[i,j,1]
				result[i,j,2]=gray[i,j,2]			
		else :
			result[i,j,0]=gray[i,j,0]
			result[i,j,1]=gray[i,j,1]
			result[i,j,2]=gray[i,j,2]			




cv2.imwrite('result'+str(THRESHOLD)+'.jpg',result)
cv2.waitKey()



