import random
from turtle import color
#for macbookpro
import matplotlib as mp
mpl.use('tkagg')

from matplotlib import pyplot as plt
import matplotlib.colors as mcolors
import matplotlib.animation as animation
import datetime
from matplotlib import colors

class GameOfLife:
    def __init__(self, shape=3, timeSteps = 100, mode="random"):

        self.shape = shape
        self.timeSteps = timeSteps
        self.frame_processed = 0
        self.state = [[0 for i in range(shape)] for j in range(shape)]
        # kernel to sum the neighbours
        self.kernel = [[1, 1, 1],
                       [1, 0, 1],
                       [1, 1, 1]]
        # random.seed(42)
        # different selection mode to check
        if mode == "random":
            self.random_starting_grid()
        elif mode == "blinker":
            self.blinker()
        elif mode =="block":
            self.rectangular_block()
        elif mode =="glider":
            self.glider()
        elif mode =="glidergun":
            self.glider_gun()
        elif mode =="qbs":
            self.queen_bee_shuttle()
        elif mode =="hwss":
            self.hwss()

        self.dead_cells = 0
        self.live_cells = sum(sum(self.state,[]))  
        self.startTime = datetime.datetime.now()

    def pad_frame_once(self, pad):
        output = [[pad, *line, pad] for line in self.state]
        return [[pad] * len(output[0]), *output, [pad] * len(output[0])]

    def update_state(self):
        """
        convolve kernel and apply game of life logic to find number of neighbours
        updates the state of the board game at each step
        """
        temp_state = [[0 for i in range(self.shape)] for j in range(self.shape)]
        self.frame_processed +=1 #record the number of frames procedded
        padded_state = self.pad_frame_once(0) #Perform the padded operation so that convolution will be of same size
        for i in range(len(padded_state)-2):
            for j in range(len(padded_state)-2):
                neighbors = (self.kernel[0][0]*padded_state[i][j] + self.kernel[0][1] * padded_state[i][j+1] + self.kernel[0][2] * padded_state[i][j+2] + 
                            self.kernel[1][0] * padded_state[i+1][j] + self.kernel[1][1]* padded_state[i+1][j+1] + self.kernel[1][2]*padded_state[i+1][j+2] +
                            self.kernel[2][0] * padded_state[i+2][j] + self.kernel[2][1]*padded_state[i+2][j+1]+ self.kernel[2][2]*padded_state[i+2][j+2])
                # print(neighbors,end=" ")
                if ((neighbors < 2) | (neighbors > 3)): #case where cells are solitude or over populated kill the cell
                    temp_state[i][j] = 0
                    if self.state[i][j] == 1 : #check if it was alive before 
                        self.dead_cells +=1
                
                if ((neighbors ==2)|(neighbors ==3)) & (self.state[i][j] == 1): # For a space that is populated, Each cell with two or three neighbors survives.
                    temp_state[i][j] = 1
                
                if neighbors ==3: #For a space that is empty or unpopulated: Each cell with three neighbors becomes populated
                    temp_state[i][j] = 1
        self.state = temp_state
        self.live_cells += sum(sum(self.state,[])) 

        if (self.frame_processed >= self.timeSteps):
            game.print_stats()
            plt.close()
            exit()


    def random_starting_grid(self, density=0.3):
        self.state = [[int(random.random() < density)
                       for i in range(self.shape)] for j in range(self.shape)]

    def blinker(self):
        self.state[1][0] = 1
        self.state[1][1] = 1
        self.state[1][2] = 1
    
    def rectangular_block(self):
        self.state[1][1] = 1
        self.state[1][2] = 1
        self.state[2][2] = 1
        self.state[2][2] = 1

    def glider(self):
        self.state[3][1] = 1
        self.state[3][2] = 1
        self.state[3][3] = 1
        self.state[2][3] = 1
        self.state[1][2] = 1

    def glider_gun(self):
        self.state[6][1]  = 1
        self.state[7][1]  = 1
        self.state[6][2]  = 1
        self.state[7][2]  = 1
        self.state[6][11] = 1
        self.state[7][11] = 1
        self.state[8][11] = 1
        self.state[5][12] = 1
        self.state[9][12] = 1
        self.state[10][13] = 1
        self.state[4][13] = 1
        self.state[4][14] = 1
        self.state[10][14] = 1
        self.state[7][15] = 1
        self.state[5][16] = 1
        self.state[9][16] = 1
        self.state[6][17] = 1
        self.state[7][17] = 1
        self.state[8][17] = 1
        self.state[7][18] = 1
        self.state[4][21] = 1
        self.state[5][21] = 1
        self.state[6][21] = 1
        self.state[4][22] = 1
        self.state[5][22] = 1
        self.state[6][22] = 1        
        self.state[3][23] = 1
        self.state[7][23] = 1 
        self.state[2][25] = 1
        self.state[3][25] = 1        
        self.state[7][25] = 1
        self.state[8][25] = 1
        self.state[4][35] = 1
        self.state[5][35] = 1         
        self.state[4][36] = 1
        self.state[5][36] = 1

    def queen_bee_shuttle(self):
        self.state[3][1] = 1
        self.state[4][1] = 1
        self.state[3][2] = 1
        self.state[4][2] = 1
        self.state[8][6] = 1
        self.state[8][7] = 1
        self.state[8][8] = 1
        self.state[7][17] = 1
        self.state[8][17] = 1
        self.state[9][17] = 1
        self.state[7][18] = 1
        self.state[7][19] = 1
        self.state[8][19] = 1
        self.state[9][19] = 1
        self.state[8][28] = 1
        self.state[8][29] = 1
        self.state[8][30] = 1
        self.state[3][34] = 1
        self.state[4][34] = 1
        self.state[3][35] = 1
        self.state[4][35] = 1

    def hwss(self):
        self.state[33][31] = 1
        self.state[34][31] = 1
        self.state[35][31] = 1
        self.state[32][32] = 1
        self.state[35][32] = 1
        self.state[35][33] = 1
        self.state[31][34] = 1
        self.state[35][34] = 1
        self.state[31][35] = 1
        self.state[35][35] = 1
        self.state[35][36] = 1
        self.state[32][37] = 1
        self.state[34][37] = 1

    def frame_plotter(self, _):
        self.update_state()
        self.ax.clear()
        cmap = colors.ListedColormap(['white','blue'])
        self.ax.imshow(self.state,cmap=cmap)
        self.ax.set(xticklabels=[], yticklabels=[])
        self.ax.tick_params(bottom=False, left=False)


    def animate(self):
        self.fig, self.ax = plt.subplots(figsize=(12, 12), dpi=self.shape+3)

        ani = animation.FuncAnimation(
            self.fig, self.frame_plotter, frames=range(30), interval=0, cache_frame_data=False
        )
        plt.show()

    def print_stats(self):
        endTime=datetime.datetime.now()
        print("Simulation Start time = ",self.startTime)
        print("Simulation End time = ",endTime)
        print("Execution time in milliseconds = ",1000*(endTime - self.startTime).total_seconds())
        print("No of frames procedded = ",self.frame_processed)
        print("Average frame procedded per second = ", self.frame_processed/(endTime - self.startTime).total_seconds() )
        print("No of alive cells = ",self.live_cells)
        print("No of dead cells = ", self.dead_cells)



if __name__ == "__main__":
    modes = ["random","blinker","block","glider","glidergun","qbs","hwss"]
    gridSize = int(input("Enter the size of the grid( between 50-200): "))
    step = int(input("Enter time steps/total frames for the simulation to perform: "))
    print("Please Choose the intial state to get started from the follwing")
    for index,mode in enumerate(modes):
        print(index,mode)
    selection = int(input("Please select the initial state from above number:"))
    if selection >=len(modes) or selection <0:
        selection = 0
    if gridSize < 50 :
        gridSize = 50
        
    game = GameOfLife(shape=gridSize, timeSteps = step, mode=modes[selection])
    game.animate()
