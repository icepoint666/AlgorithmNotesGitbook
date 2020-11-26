# 归并排序题目

**题目**

<table>
  <thead>
    <tr>
      <th style="text-align:left">&#x5E8F;&#x53F7;/&#x96BE;&#x5EA6;</th>
      <th style="text-align:left">&#x540D;&#x5B57;</th>
      <th style="text-align:left">&#x5907;&#x6CE8;</th>
      <th style="text-align:left"></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left">&#x5251;&#x6307; Offer 51</td>
      <td style="text-align:left">&#x6570;&#x7EC4;&#x4E2D;&#x7684;&#x9006;&#x6570;&#x5BF9;</td>
      <td style="text-align:left">&#x5F52;&#x5E76;&#x6392;&#x5E8F;</td>
      <td style="text-align:left">&#x4E2D;&#x7B49;</td>
    </tr>
    <tr>
      <td style="text-align:left">148</td>
      <td style="text-align:left">&#x6392;&#x5E8F;&#x94FE;&#x8868;</td>
      <td style="text-align:left">
        <p>&#x8981;&#x6C42;&#x65F6;&#x95F4;&#xFF1A;O(NlogN)&#xFF0C;&#x7A7A;&#x95F4;&#xFF1A;&#x5E38;&#x6570;</p>
        <p>&#xFF08;&#x5176;&#x5B9E;&#x8FD9;&#x91CC;&#x5B9E;&#x73B0;&#x7684;&#x5F52;&#x5E76;&#xFF0C;&#x6CA1;&#x6709;&#x505A;&#x5230;&#x7A7A;&#x95F4;&#x4E3A;&#x5E38;&#x6570;&#x8FD9;&#x4E00;&#x70B9;&#xFF09;</p>
      </td>
      <td style="text-align:left">&#x4E2D;&#x7B49;</td>
    </tr>
  </tbody>
</table>

**剑指 Offer 51. 数组中的逆序对**

归并排序merge的时候，后半段的数如果要往前排时

计算前半段剩余比它大的数有几个，然后这个个数记在count上
