liam
====
"""
Liam Friar
cosc101L - C
Lab 11
April 16, 2013
"""

def Open_and_Store():
    """Opens file and stores pertinent information in a dictionary"""
    fin = open("periodictable.txt")
    periodic_table = {}
    for line in fin:
        linesplit = line.split(",")
        periodic_table[linesplit[1]] = [linesplit[5],float(linesplit[11])]
    fin.close()
    return periodic_table
    #Dictionary entries with atomic symbol as key, and a value
    #of a list with index 0 the element name as a strin
    #and index 1 the atomic weight as a float.


def Take_Input(periodic_table):
    """Takes user input, validates response, modifies entry to be used
in later functions."""

    #Will break only after a valid input.
    formula = raw_input("Enter a chemical formula or 'done' to quit (example formula: H2-O): ")
    if formula == "done":
        return formula  #Tells main functionuser is finished.
    else:
        element_split = formula.split("-")   #Splits formula into list of element symbols with their multipliers
        element_list = []
        for i in range(len(element_split)):
            element = Validate_Input(element_split[i],periodic_table)
            if element == False:
                return "invalid"   #Will tell main function to reprimand user and recall Take_Input()
            else:
                element_list += [element]
        return element_list


def Validate_Input(element,periodic_table):
    """Validates that an input other than 'done' is in valid molecular format. Function input is each item in a list of strings from
the user input, split by '-'. Each of these function inputs should consist of a valid atomic symbol, followed by an integer (an uninterrupted
series of characters from '0' to '9'. If valid, this function returns a list with index 0 as the atomic symbol and index 1 as the
multiplier. If invalid, this function returns False which will cause the Take_Input loop to break and start again."""


    element_symbol = ""
    element_multiplier = ""
    index = 0
    while index < len(element) and element[index].isalpha() == True:  #loop finds reconstructs the element symbol without multiplier as 'element_symbol'
        element_symbol += element[index]
        index += 1
    if element_symbol not in periodic_table:
        return False              #Not a valid formula
    while index < len(element):
        if ord(element[index]) in range(48,58):   #Character from '0' to '9'
            element_multiplier += element[index]   #I reconstruct the multiplier as a string because it allows me to check character by character that this is actually an integer.
        else:
            return False   #There was a non-numeric character. See comments earlier in while loop.
        index += 1
    if element_multiplier == "":   #If no explicit multiplier, 1 is implied.
        element_multiplier = "1"
    element_multiplier = int(element_multiplier)
    element_list = [element_symbol,element_multiplier]
    return element_list


def Element_Names(element_list,periodic_table):
    """prints the name of each of the elements whose symbol appears in element_list, using dictionary 'periodic_table' Returns None """
    for element in element_list:
        print element[0],"is",periodic_table[element[0]][0]
    return


def Atomic_Weight(element_list,periodic_table):
    """computes and prints the atomic weight of the total compound whose elements are listed in element_list. Returns None"""
    weight = 0
    for element in element_list:
        weight += periodic_table[element[0]][1]*element[1]  #element's weight*multiplier
    print "The atomic weight of this compund is:",weight
    return



def main():
    """User enters a chemical formula in the form of H2-O. Program parses out formula and tells user the name of each element and the atomic
weight of the total compound. To exit program, user must enter 'done' when prompted for a formula"""

    periodic_table = Open_and_Store()
    print "Meet the elements!"                  #Only happens once, so not in subfunctions which may loop.
    while True:
        element_list = Take_Input(periodic_table)
        if element_list == "done":
            print "I am elementally confused that you want to quit, but okay."
            return         #Ends loop, ends function
        elif element_list == "invalid":
            print "Invalid user input."
        else:
            Element_Names(element_list,periodic_table)
            Atomic_Weight(element_list,periodic_table)
            print #Line to separate rounds of function
            

main()
