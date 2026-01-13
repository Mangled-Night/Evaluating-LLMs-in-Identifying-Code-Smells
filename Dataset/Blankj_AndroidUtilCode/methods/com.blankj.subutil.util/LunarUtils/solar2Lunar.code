/** 
 * 公历转农历
 * @param solar 公历
 * @return 阴历
 */
public static Lunar solar2Lunar(final Solar solar){
  Lunar lunar=new Lunar();
  int index=solar.solarYear - SOLAR_1_1[0];
  int data=(solar.solarYear << 9) | (solar.solarMonth << 5) | (solar.solarDay);
  int solar11=0;
  if (SOLAR_1_1[index] > data) {
    index--;
  }
  solar11=SOLAR_1_1[index];
  int y=getBitInt(solar11,12,9);
  int m=getBitInt(solar11,4,5);
  int d=getBitInt(solar11,5,0);
  long offset=solarToInt(solar.solarYear,solar.solarMonth,solar.solarDay) - solarToInt(y,m,d);
  int days=LUNAR_MONTH_DAYS[index];
  int leap=getBitInt(days,4,13);
  int lunarY=index + SOLAR_1_1[0];
  int lunarM=1;
  int lunarD=1;
  offset+=1;
  for (int i=0; i < 13; i++) {
    int dm=getBitInt(days,1,12 - i) == 1 ? 30 : 29;
    if (offset > dm) {
      lunarM++;
      offset-=dm;
    }
 else {
      break;
    }
  }
  lunarD=(int)(offset);
  lunar.lunarYear=lunarY;
  lunar.lunarMonth=lunarM;
  lunar.isLeap=false;
  if (leap != 0 && lunarM > leap) {
    lunar.lunarMonth=lunarM - 1;
    if (lunarM == leap + 1) {
      lunar.isLeap=true;
    }
  }
  lunar.lunarDay=lunarD;
  return lunar;
}
