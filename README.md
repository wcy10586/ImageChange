# 关于图形处理的一些笔记
＃＃ 使用canvas  的clip方法处理图形


        Path mPath = new Path();
        canvas.drawColor(Color.GRAY);
        canvas.save();
        canvas.translate(10, 10);
        drawScene(canvas);
        canvas.restore();


        canvas.save();
        canvas.translate(160, 10);
        canvas.clipRect(10, 10, 90, 90);
        canvas.clipRect(30, 30, 70, 70, Region.Op.XOR);//取它与前面结果交集的补集
        drawScene(canvas);
        canvas.restore();


        canvas.save();
        canvas.translate(10, 160);
        mPath.reset();
        mPath.cubicTo(0, 0, 100, 0, 100, 100);
        mPath.cubicTo(100, 100, 0, 100, 0, 0);
        canvas.clipPath(mPath, Region.Op.REPLACE);//替换,无论前面怎么取集合,结果都以RELPLACE的rect为准
        drawScene(canvas);
        canvas.restore();

        canvas.save();
        canvas.translate(160, 160);
        canvas.clipRect(0, 0, 50, 50);
        canvas.clipRect(60, 60, 100, 100, Region.Op.UNION);//取它与前面结果并集
        drawScene(canvas);
        canvas.restore();

        canvas.save();
        canvas.translate(10, 310);
        canvas.clipRect(0, 0, 60, 60);
        canvas.clipRect(40, 40, 100, 100, Region.Op.XOR);//取它与前面结果交集的补集
        drawScene(canvas);
        canvas.restore();

        canvas.save();
        canvas.translate(160, 310);
        canvas.clipRect(0, 0, 60, 60);
        canvas.clipRect(40, 40, 100, 100, Region.Op.REVERSE_DIFFERENCE);//取它和与前面结果交集的在当前rect中的补集
        drawScene(canvas);
        canvas.restore();


        canvas.save();
        canvas.translate(10, 460);
        canvas.clipRect(0, 0, 60, 60);
        canvas.clipRect(40, 40, 100, 100, Region.Op.DIFFERENCE);//取它和与前面结果交集的在前一个rect中的补集
        drawScene(canvas);
        canvas.restore();

        canvas.save();
        canvas.translate(160, 460);
        canvas.clipRect(0, 0, 90, 90);
        canvas.clipRect(40, 40, 100, 100, Region.Op.INTERSECT);//取它和与前面结果的交集
        drawScene(canvas);
        canvas.restore();

        //注意,如果前面没有做任何Rect的操作,那么,前面结果即是默认使用的rect.
        
        
＃＃＃ 使用matrix处理图片
   操作顺序应该写作：
    matrix.setXX();
    matrix.prepare XX（）
    matrix.postXX();
   它们的执行顺序为 pre－》set －》 post
  
  重点方法matrix.setValues(values);values是一个3X3的矩阵。矩阵不同的参数代表了不同的操作。
  matrix的坐标原点默认为canvas的原点，即左上角，向右为x正方向，向下为y正方向。图片的左上角默认与原点重合。
    
    1.平移
  （1，0，a，
    0，1，b，
    0，0，1）表示对图片做平移操作，x方向的移动距离为a，y方向移动距离为b。因为都是相对于（0，0）点所以可以看作移动到（a，b）点
    
    
  2.旋转（cos r，－sin r， -acos r+bsin r + a,
          sin r,   cos r,  -asin r-bcos r + b,
          0,        0,        1）表示绕 （a，b）点旋转 r的角度
    
  3.错切（1，b，0，
          d，1，0，
          0，0，1）错切矩阵。
  4.对称变化 （b*b - a*a ,     -2ab  , -2ac,
                  -2ab   ,  a*a - b*b, -2bc,
                    0    ,       0   ,   1）矩阵系数为：1/(a*a + b*b)；这是y以求直线ax＋by＋c ＝ 0为堆成轴的图形。
                    
  5.中心对称 （－1， 0 ，a，
                 0，－1，b，
                 0， 0 ，1）以（a，b）点为对称中心的对称图形
