unsigned int tempMin = 19;                 //红灯亮温度
unsigned int tempMax = 22;                 //报警温度
void setup( ) 
{
  Serial.begin(115200);                    //串口初始化
  pinMode(A0, INPUT); 
  pinMode(A2, INPUT); 
  pinMode(10, OUTPUT);
  pinMode(8, OUTPUT);
  pinMode(12, OUTPUT);
  digitalWrite(12, LOW);
  digitalWrite(10, LOW);
  digitalWrite(8, LOW);
}

void loop( )
{
  int n1 = analogRead(A2);
  int n2 = analogRead(A0);
  double temp1 = n1 * (5.0 / 1024.0*100);  //使用双精度浮点数存储温度数据，温度数据由电压值换算得到
  Serial.println(temp1);                   //串口输出温度数据
  double temp2 = n2 * (5.0 / 1024.0*100);    
  Serial.println(temp2);                  
  double Value=abs(temp1-temp2);           //计算温差
 if (temp1 <= tempMin)                      //小于亮灯门限值
{         
        digitalWrite(8, LOW);
        digitalWrite(12, LOW);
      }
    else if (temp1 < tempMax)               //小于报警门限
 {   
         digitalWrite(8, LOW);
         digitalWrite(12, HIGH);
       }
    else                                   //发光二极管亮度最大值，并启动声音报警
{          
          digitalWrite(8,HIGH);
          digitalWrite(12,HIGH);
        }
  if(Value>=1)                             //温差大于1，绿灯亮
    {
        digitalWrite(10, HIGH);
  }
  else{
        digitalWrite(10,LOW);
  }
  Serial.print("InsideTemp: ");  
  Serial.print(temp1);
  Serial.print(" Degrees    OutsideTemp: ");  
  Serial.print(temp2);
  Serial.println(" Degree");
  Serial.print(" Temp Difference: ");
  Serial.println(Value);
  delay(1000);                              // 控制刷新速度
}
