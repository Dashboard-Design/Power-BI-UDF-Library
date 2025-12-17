DEFINE
    /// Returns a color based on Actual–Comparison: "Normal" = positive good, "Reverse" = positive bad, and "Transparent" = always transparent.
    FUNCTION ColorCF =
        (
            Actual       : numeric,
            Comparison   : numeric,
            ColorCondition : string
        ) =>
		SWITCH(
			TRUE(),
			ColorCondition = "Normal"  && Actual - Comparison < 0, "#FA0103",
			ColorCondition = "Normal"  && Actual - Comparison > 0, "#69BA16",
			ColorCondition = "Reverse" && Actual - Comparison < 0, "#69BA16",
			ColorCondition = "Reverse" && Actual - Comparison > 0, "#FA0103",
			Actual - Comparison = 0, "black",
			ColorCondition = "Transparent", "rgba(0,0,0,0)",
			ERROR("ColorCF: Invalid inputs provided.")
		)
