# calculate a database
#' data frame calculation
#'
#' @param d1 the starting thickness (mm). default: 0.01 mm
#' @param d2 the ending thickness (mm). default: 10 mm
#' @param step the step that used to calculate the data. default: 0.1 mm. Warning: a step less than 0.1 mm would greatly increase the calculation time.
#' @param data a data frame that need to be calculated
#'
#' @return a dataframe containing the calculated reflections loss (dB) as functions of frequency and thickness
#' @export
#'
#' @examples
#' calc_EM_data (d1 = 1, d2 = 10, step = 0.5, data = temp)
#' @description Convert the raw data of complex permeability and permittivity into a data frame with calculated reflection loss as functions of frequency and thickness
calc_EM_data <- function (d1=0.01,d2=10, step = 0.1, data) {
  df_RD <- data.frame(x=numeric(0),y=numeric(0),z=numeric(0))
  cal_df <- function (d,data=data) {
    c <- 3*10^8
    for (j in 1:nrow(data)) {
      f <- as.numeric(data[j,1]) #frequency in GHz
      e1 <- as.numeric(data[j,2]) #real part of complex permittivity
      e2 <- as.numeric(data[j,3]) #imaginary part of complex permittivity
      u1 <- as.numeric(data[j,4]) #real part of complex permeability
      u2 <- as.numeric(data[j,5]) #imaginary part of complex permeability
      tane <- e2/e1 #dielectric loss tangent
      tanu <- u2/u1 #magnetic loss tangent
      u <- complex(real = u1,imaginary = 0-u2)
      e <- complex(real = e1,imaginary = 0-e2)
      if (f<100) {
        tem <- (2*pi*f*10^9*d/1000/c)*sqrt(u*e)
      } else {
        tem <- (2*pi*f*d/1000/c)*sqrt(u*e)
      }
      Z <- sqrt(u/e)*tanh(tem*complex(real=0,imaginary = 1))
      R <- 20*log10(Mod((Z-1)/(Z+1)))
      df_RD <- rbind(df_RD,c(f,R,d,e1,e2,u1,u2,tane,tanu))
    }
    colnames(df_RD) <- c("f","R","d","e1","e2","u1","u2","tane","tanu")
    return (df_RD)
  }
  d <- seq(as.numeric(d1),as.numeric(d2),0.1)
  df <- map_df(d, cal_df,data)
  return (df)
}



#calculate the bandwidth of RL < -*dB-----
#' bandwidth calculation
#'
#' @param d1 the starting thickness, default 0.01 mm
#' @param d2 the end thickness, default 10 mm
#' @param RL the RL that used to calculate the bandwidth, default -10 dB
#' @param data the data that need to be calculated
#'
#' @return a data frame with thickness and corresponding bandwidth
#' @export
#'
#' @examples
#' EM_bandwidth(d1=1,d2=5,RL=05,data=temp)
#' @description calculate the bandwidth of RL< -*dB at the assigned thickness range
EM_bandwidth <- function (d1 = 0.01,d2 = 10,RL = -10,data) {
  df <- calc_EM_data(d1,d2,data = data)
  df_bandwidth <- df %>%
    group_by(d) %>%
    filter(R<RL) %>%
    summarise(width=(max(f)-min(f)))

  return (df_bandwidth)

}

#calculate the maximum reflection loss-----
#' maximum RL
#'
#' @param d1 the starting thickness, default 0.01 mm
#' @param d2 the end thickness, default 10 mm
#' @param data the data that need to be calculated
#'
#' @return a data frame with maximum RL value at each thickness
#' @export
#'
#' @examples
#' EM_maxRL(d1=1,d2=5,data=temp)
#' @description calculate the maximum RL
EM_maxRL <- function (d1=0.01,d2=10,data) {
  df <- calc_EM_data(d1,d2,data = data)
  df_maxRL <- df %>%
    group_by(d) %>%
    summarise(min=min(R))
  #variation of maximum RL as a function of thickness
  return(df_maxRL)
}

