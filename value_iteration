import numpy as np
import random as rand

def _value_iteration(shape,_terminal_mask,_reward_grid):
        U1 = np.zeros([shape,shape])
        direction = np.zeros([shape,shape],dtype='int')
        
        R,  gamma = _reward_grid, 0.9
        epsilon=0.1
        
        while True:
            U = U1.copy()
            delta = 0
            for i in range(shape):
                for j in range(shape):
                    max1=[]
                    if (_terminal_mask[i][j]==True):
                        max1.append(0)
                    else:
                    
                    #When (i,j) and action is UP
                        k2=0
                        k1=0
                        m1=0
                        m2=0
                        if((i+1)>shape-1):
                            k2=i
                        else:
                            k2=i+1
                        if((i-1)<0):
                            k1=i
                        else:
                            k1=i-1
                        if((j+1)>shape-1):
                            m2=j
                        else:
                            m2=j+1
                        if((j-1)<0):
                            m1=j
                        else:
                            m1=j-1
                        #print("K1",k1,"k2",k2,"m1",m1,"m2",m2)
                        up=0
                        up=0.7*U[k1,j]+0.1*(U[k2,j]+U[i,m1]+U[i,m2])
                        down=0
                        down=0.7*U[k2,j]+0.1*(U[k1,j]+U[i,m1]+U[i,m2])
                        right=0
                        right=0.7*U[i,m2]+0.1*(U[k1,j]+U[i,m1]+U[k2,j])
                        left=0
                        left=0.7*U[i,m1]+0.1*(U[k1,j]+U[i,m2]+U[k2,j])
                        max1.append(up)
                        max1.append(down)
                        max1.append(right) 
                        max1.append(left)
                    U1[i,j] = R[i,j] + gamma * max(max1)
                    if (_terminal_mask[i][j]==True):
                        direction[i,j]=9090
                    else:
                        maxval=max(max1)
                        #print ("Maxval",maxval)
                        list1=[]
                        for k in range(len(max1)):
                            if maxval==max1[k]:
                                list1.append(k)
                        #print("List1",list1)

                        if(len(list1)>1):
                            index=min(list1)
                        else:
                            index=list1[0]  
                        if index==0:
                            d=0
                        elif index==1:
                            d=2
                        elif index==2:
                            d=1
                        else:
                            d=3
                        direction[i,j]=d
                    delta = max(delta, abs(U1[i,j] - U[i,j]))
            #if delta < epsilon * (1 - gamma) / gamma:
            if delta < epsilon * (1 - gamma) / gamma:
                #print ("Utility",U)
                #print("Direction",direction)
                        #print(U.reshape(self.shape))
                return U,direction
def no_turn(move,p):
    move1=move
    if p[move]==0:
        move=list(move)
        move[0]+=-1
        move[1]+=0    
    elif p[move]==1:
        move=list(move)
        #move= move-(-1,0)
        move[0]+=0
        move[1]+=1   
    elif p[move]==2:
        move=list(move)
        #move= move-(0,1)  
        move[0]+=1
        move[1]+=0   
    else:
        move=list(move)
        #move= move-(1,0)
        move[0]+=0
        move[1]+=-1   
    if move[0]>len(p)-1 or move[0]<0 or move[1]>len(p)-1 or move[1]<0:
        move=move1
        
    return move

orientations = NORTH, EAST,SOUTH, WEST = [(-1,0), (0, 1),(1, 0), (0,-1)]
turns = LEFT, RIGHT = (-1, 1)
def turn_heading(heading, inc, headings=orientations):
    return headings[(headings.index(heading) + inc) % len(headings)]
def turn_right(heading):
    return turn_heading(heading, RIGHT)
def turn_left(heading):
    return turn_heading(heading, LEFT)
if __name__ == '__main__':
    #To read from input file
    input_file = open("input.txt", "r")

    #s is the size of grid
    s=int(input_file.readline())

    #n is the number of cars
    n=int(input_file.readline())

    #o is the number of obstracles
    o=int(input_file.readline())

    #obstracles=[]
    
    #List containing all the obstracles
    trap=[]
    if o>0:
        
        for i in range(o):
            obstracle=input_file.readline().strip().split(',')
            trap.append((int(obstracle[1]),int(obstracle[0])))
    #print trap
        trap=list(set(trap))
        #print (trap)
    start1=[]
    if n>0:
          for i in range(n):
            position=input_file.readline().strip().split(',')
            start1.append((int(position[1]),int(position[0])))
    #print (start1)
    goal1=[]
    if n>0:
          for i in range(n):
            position=input_file.readline().strip().split(',')
            goal1.append((int(position[1]),int(position[0])))
                    
    #print (goal1)
    #print "Goal",goal
    #print "Start",start
    input_file.close()
    average1=[]
    if n==0:
        #print("No cars available")
        average1.append(0)
    else:
       
        for i in range(n):
            shape = (s,s)
            goal = goal1[i]
            start = start1[i]
            default_reward = -1
            goal_reward = 100
            trap_reward = -100
            reward_grid = np.zeros(shape) + default_reward
            reward_grid[goal]+= goal_reward
            #reward_grid[trap] = trap_reward
            for i in trap:
                reward_grid[i]+= trap_reward
            #print (reward_grid)

            terminal_mask = np.zeros_like(reward_grid, dtype=np.bool)
            terminal_mask[goal] = True

            obstacle_mask = np.zeros_like(reward_grid, dtype=np.bool)
            for i in trap:
                obstacle_mask[i] = False
        
            u1=np.zeros(shape)
            p1=np.zeros(shape)
            u1,p1=_value_iteration(shape[0],terminal_mask,reward_grid)
            #print(p1)
            #print(u1)
            p=p1
            average=[]
            for j in range(10):
                pos=start
                #print (j,"~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~")
                
                np.random.seed(j)
                swerve=np.random.random_sample(1000000)
                k=0
                count=0
                if pos==goal:
                    count+=reward_grid[pos]+1
                    #count+=reward_grid[pos]
                    #print("Count",count)
                    #print(pos)
                #
                while pos!=goal and k<1000000 :
                    move=pos
                    move1=p1[move]
                    if move1==0:
                        #UP
                        move2=(-1,0)
                    elif move1==1:
                        #right
                        move2=(0,1)
                    elif move1==2:
                        #down
                        move2=(1,0)
                    else:
                        #left
                        move2=(0,-1)
                    a=np.float64(swerve[k])
                    
                    
                    if swerve[k]>0.7:
                            if swerve[k]>0.8:
                                if swerve[k]>0.9:
                                    
                                    #move1=tuple(np.add(move,tuple(turn_right(move))))
                                    
                                    move2=turn_right(turn_right(move2))
                                    temp=tuple(np.add(move2,move))
                                    if temp[0]<0 or temp[1]<0 or temp[1]>shape[0]-1 or temp[0]>shape[0]-1:
                                        move=move
                                    else:
                                        move=temp
                                    
                                    #move=tuple(np.add(move1,tuple(turn_right(move1))))
                                else:
                                  
                                    move2=turn_right(move2)
                                    temp=tuple(np.add(move2,move))
                                    if temp[0]<0 or temp[1]<0 or temp[1]>shape[0]-1 or temp[0]>shape[0]-1:
                                        move=move
                                    else:
                                        move=temp
                                    

                            else:
                               
                                move2=turn_left(move2)
                                temp=tuple(np.add(move2,move))
                                if temp[0]<0 or temp[1]<0 or temp[1]>shape[0]-1 or temp[0]>shape[0]-1:
                                        move=move
                                else:
                                        move=temp
                                #print (move)
                                

                    else:
                        move=tuple(no_turn(move,p))
                    count+=reward_grid[move]  
                    pos=move
                        #print(move)
                    k+=1
                average.append(count)
            Average=int(np.floor(sum(average)/len(average)))
            average1.append(Average)
            #print("Average ",Average )
                #print("Utility Grid",utility_grids[:, :, -1]) #Actual

        f1=open("output.txt","w")
        for k in average1:
            f1.write("%d\n"%k)
        f1.close()
                
