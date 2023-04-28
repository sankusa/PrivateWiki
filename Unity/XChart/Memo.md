```
using System.Collections;
using System.Collections.Generic;
using UnityEngine;
using XCharts;

public class LineChartController : MonoBehaviour
{
    [SerializeField] private XCharts.Runtime.LineChart lineChart;

    public void SetData(List<double> values)
    {
        var line = lineChart.GetSerie<XCharts.Runtime.Line>();
        line.ClearData();
        foreach(double value in values)
        {
            lineChart.AddData(0, value);
        }
        values.ForEach(x => line.AddData(0, x));
    }

    // 横軸の表示(目盛り関係)は値の取る範囲によって若干変わる。表示を固定するために横軸の値はindexを0～1に正規化して使う。
    public void SetDataWithXAxisNormalization(IReadOnlyList<double> values)
    {
        var line = lineChart.GetSerie<XCharts.Runtime.Line>();
        line.ClearData();
        for(int i = 0; i < values.Count; i++)
        {
            double x = (double)i / (values.Count - 1);
            line.AddData(x, values[i]);
        }
    }

    public void ClearData()
    {
        var line = lineChart.GetSerie<XCharts.Runtime.Line>();
        line.ClearData();
    }

    public void SetYAxisMinMax(double min, double max)
    {
        var yAxis = lineChart.GetOrAddChartComponent<XCharts.Runtime.YAxis>();
        yAxis.min = min;
        yAxis.max = max;
    }
}

```
