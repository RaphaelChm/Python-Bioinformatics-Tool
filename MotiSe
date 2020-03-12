from Bio import ExPASy
from Bio import SwissProt
from Bio import SeqIO
import re
from tkinter import *
from tkinter import messagebox

#Define help button click event
def popUp():
    #print("Help Window Opened")
    
    #Open a message box containing help text.
    messagebox.showinfo("Help", "Please enter a UniProtKB Accession code (e.g. P98073) or identifier (e.g. ENTK_HUMAN) into the search bar and press the 'Search' button."
                        "\nIf a protein sequence matching is found, its accession code will appear in the 'Protein Description' Box followed by its record description. Any matching sequence motifs will be listed in the 'Matches' output"
                        " text box."
                        "\nMatches will be displayed in a numbered list (in order of found) with their motif sequence sequence, as well as the start and end position in the protein sequence of the match.")
    #print("Help Window Closed")
    

#Define search button function
def click():
    entered_text = tentry.get()                 #collects text from text entry box (the search box)
    tentry.delete(0, 'end')                     #deletes text in the search box, ready for new input
    output.config(state='normal')               #allows editing of the ouput box
    output.delete(0.0, END)                     #deletes the contents of the output box
    output.config(state='disabled')             #disables editing of the output 'Protein Description' text box
    matchOut.config(state='normal')             #enables editing of the matchOut 'Matches' text box
    matchOut.delete(0.0, END)                   #deletes previous match listings from the 'Matches' text box
    matchOut.config(state='disabled')
    
    try:
        #retrieves the UniProtKB/Swiss-Prot entry for the given accesssion code/identifier
        handle = ExPASy.get_sprot_raw(entered_text) 
        record = SwissProt.read(handle)         #creates a copy of the Swiss-Prot handle
        handle.close()                          #closes the Swiss-Prot handle
        
        #creates a description object to hold the Swiss-Prot record's description
        description = (entered_text + "\nProtein Description: \n" + record.description + "\n") 
        output.config(state='normal')           #enables editing of the 
        output.insert(END, description)         #prints the protein's Swiss-Prot description to the output box
        output.config(state='disabled')         #prevents the user from editing the text inside the output box.
        #print(description)
        sequence = record.sequence              #creates an object to hold the protein sequence
        #print(sequence)
        


        
        matchcounter=0                          #Initializes an object to count the number of motif matches

        #Use an iterater to search the protein sequence string for the regular expression representing the motif
        for match in re.finditer(r"[^P]{1}[^PKRHW]{1}[VLSWFNQ]{1}[ILTYWFN]{1}[FIY]{1}[^PKRH]{1}", sequence):

            
            run_start = match.start()           #creates an object representing the start of the matched string
            run_end = match.end()               #creates an object representing the end of the matched string
            real_start = run_start + 1          #adds +1 to the starting position of the motif sequence
            real_end = run_end + 1              #adds +1 to the end position of the motif sequence
            matchcounter = matchcounter+1       #increment the match counter by 1

            #initialize a result object for matches
            result = ("Match " + str(matchcounter) + ": " + match.group() + "  Position: (" + str(real_start) + "," + str(real_end) + ")\n")

            #Print matches
            matchOut.config(state='normal')     #Enables editing of the 'Matches' text box so that results can be printed
            matchOut.insert(END, result)        #Prints results in the 'Matches' text box
            #Disables the 'Matches' text box again to prevent the user accidentally editing results.
            matchOut.config(state='disabled')   
        else:
            
            #When no more matches are found, the for loop will switch to the 'else', printing a message
            matchOut.config(state='normal')                     #Enables editing of the 'Matches' text box
            matchOut.insert(END, "\nNo more matches found...")  #Prints a message indicating that the program has found all the matches it can
            matchOut.config(state='disabled')                   #Disables the 'Matches' text box again
            
    except:
        #in the case of an error an exception will occur, printing a message to the output text box
        #print("Protein not found, please try another acquisition code. If problems persist, please check your internet connection and the status of Prosite and UniProt servers.")
        output.config(state='normal')                               
        output.insert(END, "You Searched: " + entered_text + "\nProtein not found, please try another acquisition code." +
                      "If problems persist, please check your internet connection and the status of Prosite and UniProt servers.")
        output.config(state='disabled')


    
#create window
window = Tk()
window.title("MotiSe")
window.configure(background='#878c92')
window.resizable(False, False)
#create label
L1 = Label(window, text="Protein Accession Code: ", background='#878c92', font=('Helvetica', 10)) .grid(row = 0, column = 0, sticky = W, padx=10, pady=10)
#L1.configure(background='#878c92')

#create input text box
tentry = Entry(window, width = 20, bg = "white")
tentry.grid(row = 0, column = 1, sticky=W)

#create submit button
b1 = Button(window, text="Search", width = 6, command = click) .grid(row = 0, column = 2, padx = (10), pady=10)


#help button
b2 = Button(window, text="help", width = 6, command = popUp) .grid(row = 1, column = 0, padx = (10), pady = 3, sticky = W)

#output label
L2 = Label(window, text = "Protein Description: ", background='#878c92', font=('Helvetica', 11)) .grid(row = 2, column = 0, sticky = W, padx = (10))

#create ouput text box
output = Text(window, width = 51, height = 10, wrap = WORD, background ='#c9d2df', highlightthickness=0, font=("Courier", 8))
output.grid(row = 3, column = 0, columnspan = 4, padx = (10), pady = (10), sticky = W)


#create label for search result box
L3 = Label(window, text = 'Matches: ', background='#878c92', font=("Helvetica", 11)) .grid(row=4, column=0, sticky=W, padx=(10))

#create second output text box for search results

matchOut = Text(window, width = 45, height = 30, wrap = WORD, background ='#c9d2df', highlightthickness=0)
matchOut.grid(row = 5, column = 0, columnspan = 4, padx = (10), pady = (10), sticky = W)
#create third text box to display number of matches


#run the main loop
window.mainloop()
