[![Gitpod ready-to-code](https://img.shields.io/badge/Gitpod-ready--to--code-blue?logo=gitpod)](https://gitpod.io/#https://github.com/junlong-gao/tax-tips)

IRS❤️CRA, 2020-2021

# General Guides
1. IRS和CRA有相互report的duty[1]，在一侧违反tax另一侧是会收到report的，IRS❤️CRA
2. tax duty和tax withholding是两回事，童鞋们应该在国境之间反复横跳的时候和公司商量好tax withholding，否则最后file的时候要么远远低于tax duty导致penalty，或者withhold了太多产生一笔巨大的refund
3. rsu在跨国tax非常复杂，特别注意一不小心withhold多了就相当于买了自己公司的put option
4. 其实double tax并没有那么糟糕，US和Canada都有foreign tax credit来offset掉本国之外收入的tax duty，毕竟你在另一个国家已经要tax duty了。但是因为总income要report，还是会影响你的tax bracket。而且如果两国tax rate非常不一样，一个国家的tax duty会产生一定量的多pay tax。

# Canada
1. 在加拿大的tax duty是否需要报global income，是否需要报全年还是入境那段时间的global income 取决于你的resident status (Resident, Non-Resident and Part-year Resident Status) [2][4] 和 The Sojourner Rule[3]
## Departure Tax
## Capital Gain/Loss and Deemed Acquisition
## Foreign Tax Credit

# US
1. 和加拿大一样，global income 按照你在US的resident status （注意US的resident status计算比较复杂，会往前推两年)
## Foreign Tax Credit
## State Tax Rules
### California: Part-year resident
> If you lived inside or outside of California during the tax year, you may be a part-year resident.
>
> As a part-year resident, you pay tax on:
>
> * All worldwide income received while a California resident
> * Income from California sources while you were a nonresident

cf. [7]

# RSU
RSU 的 tax duty 是取决你在这个国家的tax status duration。比如3月在加拿大有一笔grant，11月回到US payroll，来年3月vest，那么vest的这部分前3-11月有加拿大的tax duty，剩下的有US的tax duty。当然了，公司的会计可能不懂，就直接给你double withhold了，所以你需要file t1213来让公司不要double withhold （see below）
## Example:
Let V be a vesting, and `t_grant` be the time it was granted and `t_vest (t_now)` be the time it is vested.
Let `t_ca \in [t_grant, t_vest)` be the time one enters Canada.

Then let
`alpha=(t_ca - t_grant)/(t_vest - t_grant)`, `beta = 1 - alpha`.

The tax withholding of this vest V is then
1. US: `alpha * V * r_usa`
2. CA: `alpha * V * r_ca + beta * V * r_ca`

Usually `r_usa = r_usa_fed + r_usa_state`, and `r_ca = 53.53%`

The net withholding (without T1213) is

```
alpha * V * (r_usa + r_ca) + beta * V * r_ca
```
These taxs are harsh (can it even exceed `1 * V`?), but part of it will be returned, see below.

When file tax, the US portion is

```
alpha * V * r_usa_effective
```

the diff with the withholding is the return, where r_usa_effective is determined by your US tax status.

The Canada portion is
aThe Canada portion is
```
beta * V * r_ca_effective + alpha * V * r_ca_effective -
      min(alpha * V * r_usa_effective, alpha * V * r_ca_effective
```

The subtracted portion (which is the diff on Canada tax return) is by US-Canada tax treaty to avoid double tax (a.k.a foreign tax credit by Canada).

Assume (usually) `r_ca_effective >= r_usa_effective`, the net effect is, V is taxed by

```
V * r_ca_effective
```

And net total net return is:

```
(alpha * V * (r_usa + r_ca) + beta * V * r_ca) - V * r_ca_effective
= V * (r_ca - r_ca_effective) + alpha * V * (r_usa)
= (alpha * r_usa + r_ca - r_ca_effective) * V
```
In other words, return the usual withhold rate diff `r_ca - r_ca_effective`
and the double tax portion `alpha * r_usa`.

Both of them can be quite significant, and file T1213 will reduce the second
double tax portion.

To make you feel better, think about the company outlook: the returned portion
is effectively sign a portion of `alpha * r_usa + r_ca - r_ca_effective` as a put
option contract: these part will be returned in a year regardless company stock
outlook. So part of your vest is risk-free for loss, but neither can you enjoy the gain.
To have a feeling of how large it is, this is roughly `0.7 * 0.33 + (0.53 - 0.35) = 0.41`,
so you elect 41% of your vest to be sold on spot to mitigate the risk, the next 35%
to CRA, then finally 24% remains on vest day for growth(or loss, depending on
your company outlook).

If you have a good outlook of your company or want to take risks, get T1213
and remove `alpha * r_usa` part. Otherwise sit back and wait for it to be
returned.

# Tax Withhold
1. 在加拿大，你需要file t1213，然后让CRA approve之后交给公司来减少double withhold

# References
1. [Reporting and sharing of financial account information with the United States](https://www.canada.ca/en/revenue-agency/services/tax/international-non-residents/enhanced-financial-account-information-reporting/reporting-sharing-financial-account-information-united-states.html)
2. [Determining your residency status](https://www.canada.ca/en/revenue-agency/services/tax/international-non-residents/information-been-moved/determining-your-residency-status.html)
3. [When Do You Become a “Resident in Canada” for Income-Tax Purposes?](https://taxpage.com/articles-and-tips/part-year-residence/)
4. [Deemed residents](https://www.canada.ca/en/revenue-agency/services/tax/international-non-residents/individuals-leaving-entering-canada-non-residents/deemed-residents.html)
5. [T1213 Request to Reduce Tax Deductions at Source](https://www.canada.ca/en/revenue-agency/services/forms-publications/forms/t1213.html)
6. [Foreign Persons](https://www.irs.gov/individuals/international-taxpayers/foreign-persons)
7. [Part-year resident and nonresident](https://www.ftb.ca.gov/file/personal/residency-status/part-year-and-nonresident.html)
