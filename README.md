# Character-Recogniser
using System;
using System.Drawing;
using System.IO;
namespace detect_chars
{
    class Program
    {        
        static void examples(int[,] X, int[,] X_transpose, string folderPath, int k, int[,] Y)
        {

            string[] filePaths = Directory.GetFiles(folderPath);
            int example = 0;
            foreach(string imgPath in filePaths)
            {
                string file_name = imgPath.Substring(imgPath.LastIndexOf(@"\") + 1);
                int output = file_name[0] - '0';
                int i = 0;
                Bitmap image1 = new Bitmap(imgPath, true);
                Size size = new Size(32, 32);
                Bitmap image2 = new Bitmap(image1, size);
                for(int x=0; x < image2.Width ; x++)
                {
                    for(int y = 0; y < image2.Height; y++)
                    {
                        var red = image2.GetPixel(x, y).R * 0.2126;
                        var green = image2.GetPixel(x, y).G * 0.7152;
                        var blue = image2.GetPixel(x, y).B * 0.0722;

                        int representation = red + green + blue > 128 ? 1 : -1;
                        X[example, i] = representation;
                        X_transpose[i, example] = representation;
                        i++;
                    }
                }

                for (int training_example = 0; training_example < k; training_example++)
                {
                    Y[example, training_example] = output == training_example ? 1 : 0;
                }
                example++;
            }

        }


        static void Main(string[] args){
            /* training
            int m = 5000; // no. of training examples
            int n = 1024; //no.of features
            int k = 10; //no. of values y can take(no. of classes)

            Random random = new Random();

            double[,] theta = new double[n,k];
            for (int i = 0; i < n; i++)
            {
                for (int j = 0; j < k; j++)
                {
                    theta[i,j] = random.NextDouble();
                }
            }

            int[,] X = new int[m, n];
            int[,] X_transpose = new int[n, m];
            int[,] Y = new int[m, k];
            examples(X, X_transpose, @"H:\Training",k,Y);
            
            for (int training_output = 0; training_output<k; training_output++ ) {
                for (int count = 0; count < 1000; count++)
                {
                    double[,] Xtheta = new double[m,k];
                    for (int i = 0; i < m; i++)
                    {
                        double temp = 0;
                        for (int j = 0; j < n; j++)
                        {
                            temp += X[i, j] * theta[j,training_output];
                        }
                        temp = -1 * temp;
                        temp = Math.Exp(temp);
                        temp = 1 / (temp + 1);
                        Xtheta[i,training_output] = temp;
                    }
                    double[,] difference = new double[m,k];
                    for (int i = 0; i < m; i++)
                    {
                        difference[i,training_output] = Xtheta[i,training_output] - Y[i,training_output];
                    }
                    double alpha = 0.001;
                    for (int i = 0; i < n; i++)
                    {
                        double temp = 0;
                        for (int j = 0; j < m; j++)
                        {
                            temp += X_transpose[i, j] * difference[j,training_output];
                        }
                        theta[i,training_output] = theta[i,training_output] - (temp * (alpha / m));
                    }
                }
            }


             
            string[] theta_string = new string[n*k];
            int index = 0;
            for(int j = 0; j < k; j++)
            {
                for(int i = 0; i<n; i++)
                {
                    string str = theta[i,j].ToString();
                    theta_string[index++] = str;
                }
            }
            File.WriteAllLines(@"C:\Users\Abhishek\OneDrive\Desktop\theta.txt", theta_string);
            */
            /* validate
            string[] readText = File.ReadAllLines(@"C:\Users\Abhishek\OneDrive\Desktop\theta.txt");
            int n = 1024;
            int m = 5000;
            int k = 10;
            

            double[,] theta_check = new double[n,k];
            int index = 0;
            for(int i = 0; i < k; i++)
            {
                for (int j = 0; j < n; j++)
                {
                    theta_check[j,i] = Convert.ToDouble(readText[index++]);
                }
            }


            int[,] X_check = new int[m,n];
            int[,] X_check_transpose = new int[n,m];
            int[,] Y_check = new int[m,k];
            examples(X_check, X_check_transpose, @"H:\Training", k, Y_check);
            int count = 0;

            int[,] testResult = new int[k, k];
            for(int i = 0; i < k; i++)
            {
                for(int j = 0; j < k; j++)
                {
                    testResult[i, j] = 0;
                }
            }
            for (int i = 0; i < m; i++)
            {
                int expected = -1;
                double maxAns = 0;
                int maxIndex = -1;
                for (int training_output = 0; training_output < k; training_output++)
                {
                    if (Y_check[i, training_output]== 1){
                        expected = training_output;
                    }
                    double temp = 0;
                    for (int j = 0; j < n; j++)
                    {
                        temp += X_check[i, j] * theta_check[j, training_output];
                    }
                    temp = -1 * temp;
                    temp = Math.Exp(temp);
                    temp = 1 / (temp + 1);
                    //  Xthetacheck[i] = temp;
                    if (temp >maxAns)
                    {
                        maxAns = temp;
                        maxIndex = training_output;
                    }
                 //   Console.WriteLine(training_output +":  "+ temp);
                }
                testResult[expected, maxIndex]++;
                //Console.WriteLine("The given image is a : " + maxIndex);
            }

            Console.WriteLine("Work done");

            for(int i = 0; i < k; i++)
            {
                for(int j = 0; j < k; j++)
                {
                    if(testResult[i, j] < 10)
                    {
                        Console.Write(0);
                    }
                    if (testResult[i, j] < 100)
                    {
                        Console.Write(0);
                    }
                    Console.Write(testResult[i, j] + " ");
                }
                Console.WriteLine();
            }
           // Console.WriteLine(count);
            Console.ReadLine();
            */
        }
    }
}

