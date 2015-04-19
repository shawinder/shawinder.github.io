---
layout: post
title: "My First Blog Post"
date: 2015-04-19
---

My first blog post using Jekyll

{% highlight csharp linenos %}

using System;
using System.Collections.Generic;
using System.ComponentModel;
using System.Data;
using System.Drawing;
using System.Linq;
using System.Text;
using System.Windows.Forms;

namespace String_Stuff
{
    public partial class stringForm : Form
    {
      public stringForm()
      {
          InitializeComponent();
      }

private string SwitchCase(string input)
        {
            string str1 = input;
            string output = str1.ToUpper();

return output;

}


   private string Reverse(string input)
    {
        string str1 = input;
        char[] inputArray = input.ToCharArray();
        Array.Reverse(inputArray);
        string output = new string(inputArray);

return output;
  }

   private string PigLatin(string input)
   {
       // rules: words that begin with a consinant-
       //is moved to the end and "ay" is added.
       // words that begin with a vowel-
       // the sylable "ay" is added to the end of the word.
       // e.g. eagle -> eagleay, apple->appleay.
       string str1 = input;           
       string ay = "ay";
       input = inputTextBox.Text; // already a string, no need to convert
       input = input.Substring(1, input.Length - 1) + input.Substring(0, 1);
       str1 = " the word in Pig Latin is " + input + ay; // need to use Text property to display string
       string output= str1  ;
       return output;
   }                    



private string ShiftCypher(string input, int shift)
{
    //Each letter in the word is replaced by a letter some fixed number of positions down the alphabet.
    //For this lab, shift by 3. Example: program, shifted by 3 will become surjudp

string output = null;
char[] A = null;
A = input.ToCharArray();
int temp;
for (int i = 0; i < input.Length; i++)
{
    temp = (int) (A[i] + shift);
    output += (char)temp;
}


return output;
}

private string SubCypher(string input, string charsToSub)
{
    //Each letter in the word will be replaced by a letter from the corresponding position 
    //in a substitution alphabet. For this lab use this substitution alphabet: zeroabcdfghijklmnpqstuvwxy. 
    //Example: disk would become ofqh.
    string subAlpha = "zeroabcdfghijklmnpqstuvwxy";
    string str1 = input;
    string subCypher = subAlpha;



string output = "";
return output;
}



private void transformButton_Click_1(object sender, EventArgs e)
{
string input = inputTextBox.Text;

switchCaseTextBox.Text = SwitchCase(input);
reverseTextBox.Text = Reverse(input);
pigLatinTextBox.Text = PigLatin(input);
shiftTextBox.Text = ShiftCypher(input,shift);
subTextBox.Text = SubCypher(input, charsToSub);
}
}
}

{% endhighlight %}
