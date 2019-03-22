import requests
import re
from bs4 import BeautifulSoup
import xlwt
import time
import random
from xlrd import open_workbook
from xlutils.copy import copy


def get_font_dict():
    """
    获取每个字代表的偏移量
    :return:
    """
    url = 'http://s3plus.meituan.net/v1/mss_0a06a471f9514fc79c981b5466f56b91/svgtextcss/c6763031f4ca5ef21c6b079409f076b9.css'
    url = 'http://s3plus.meituan.net/v1/mss_0a06a471f9514fc79c981b5466f56b91/svgtextcss/66ed5cd66492f2f67ccf37ae948a303c.css'
    r = requests.get(url, headers=headers)
    font_list = re.findall('.*?{.*?}', r.text)
    font_dict = {}
    for font in font_list:
        # print(font)
        class_font = font.split('{')[0].replace('.', '')
        pianyi_list = re.findall('\d+', font.split('{')[1])
        num_list = [int(x) for x in pianyi_list if int(x) != 0]
        if len(num_list) == 1:
            num_list.insert(0, 0)
        font_dict[class_font] = num_list
    # print(font_dict)
    return font_dict


def get_font_place():
    """
    获取每一行对应的文字
    :return:
    """
    # url = 'http://s3plus.meituan.net/v1/mss_0a06a471f9514fc79c981b5466f56b91/svgtextcss/3617c635ce7e39c7e8fa09bb3a0ea481.svg'
    url = 'http://s3plus.meituan.net/v1/mss_0a06a471f9514fc79c981b5466f56b91/svgtextcss/145a30db44df01339efea895999d3587.svg'
    place_list = []
    id_dict = []
    r = requests.get(url, headers)
    # print(r.text)
    soup = BeautifulSoup(r.text, 'html.parser')
    # print(soup)
    text_list = soup.find_all('text')
    # text_list = soup.find_all('textpath')
    # id_list = soup.find_all('path')
    # print(len(text_list))
    for i in range(len(text_list)):
        # x = text_list[i]['textlength']
    #         # st = text_list[i].text
    #         # # print(id_list[i])
    #         # y = id_list[i]['d'].split()[1]
    #         # # print(y, st)
    #         # place_list.append({y: [x, st]})
        y = text_list[i]['y']
        text = text_list[i].text
        place_list.append({y:text})
    # print(place_list)
    return place_list


def get_ture_font(place):
    """
    获取真实的评论字
    :param place: 
    :return:
    """

    num = 0
    for i in font_place:
        for k in i:
            if int(k) > place[1]:
                # print(i)
                num = 1
                st = i[k]
                # st = i[k][1]
                # leng = i[k][0]
        if num:
            break
    # st_num = int((int(leng) - place[0]) / 14)
    st_num = int(place[0]) / 14
    # print(st_num)
    # true_font = st[len(st) - st_num]
    true_font = st[int(st_num)]
    # print(true_font)
    return true_font


if __name__ == '__main__':
    
    headers = {
        'Cookie': '相关cookie',
        'user-agent': 'MMozilla/5.0 (Windows NT 10.0; WOW64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/63.0.3239.132 Safari/537.36'
    }
    font_dict = get_font_dict()
    font_place = get_font_place()
    print(font_dict, font_place)

    workbook = xlwt.Workbook(encoding='ascii')
    worksheet = workbook.add_sheet('18143538')
    num = 0

    try:
        for page_num in range(1, 2000):
            url = 'http://www.dianping.com/shop/18143538/review_all/p{}'.format(page_num)
            id = re.search('\d\d\d\d+', url).group()
            if page_num % 2 == 0:
                headers = headers_1
            else:
                headers = headers_1
            r = requests.get(url, headers=headers)
            print(r.cookies)
            font_dict = get_font_dict()
            font_place = get_font_place()
            soup = BeautifulSoup(r.text, 'html.parser')
            # print(soup.find_all('em', {'class': 'col-exp'}), 1111111)
            # print(soup)
            li_list = soup.find('div', {'class': 'reviews-items'}).find_all('li')
            for li in li_list:
                try:
                    # print(li)
                    div = li.find('div', {'class': 'review-words Hide'})
                    if not div:
                        div = li.find('div', {'class': 'review-words'})
                    font_types = re.findall('<span class="([a-zA-Z0-9]{5,6})"></span>', str(div))
                    sm = str(div)
                    pingfen = re.search('\d', str(li.find('div', {'class': 'review-rank'}).find('span'))).group()
                    # print(li)
                    pingjia = li.find('div', {'class': 'review-rank'}).text.replace('\n', '')
                    pingjia_list = [re.sub('.*：', '', x) for x in pingjia.split() if x]
                    if li.find('div', {'class': 'review-pictures'}):
                        tupian = '有'
                    else:
                        tupian = '无'
                    xiaoguo = pingjia_list[0]
                    huanjing = pingjia_list[1]
                    fuwu = pingjia_list[2]
                    try:
                        xiaofei = pingjia_list[3]
                    except:
                        xiaofei = ''
                    try:
                        # print(li.find_all('em', {'class': 'col-exp'})[0])
                        # print(li.find_all('em', {'class': 'col-exp'})[0])
                        # print(type(li.find_all('em', {'class': 'col-exp'})[0]))
                        dianzan = str(li.find_all('em', {'class': 'col-exp'})[0]).replace('<em class="col-exp">(',
                                                                                          '').replace(')</em>', '')
                        # print(dianzan)
                        # huiying = re.findall('\(\d+\)', str(li))[0]
                    except Exception as e:
                        print(e, 11111111111)
                        dianzan = 0
                    try:
                        # print(str(li.find_all('em', {'class': 'col-exp'}))[1])
                        huiying = str(li.find_all('em', {'class': 'col-exp'})[1]).replace('<em class="col-exp">(',
                                                                                          '').replace(')</em>', '')

                    except:
                        huiying = 0
                    shijian = li.find('span', {'class': 'time'}).text.replace('\n', '').strip()
                    # print(fuwu)
                    vip = li.find('span', {'class': 'vip'})
                    # print(li)
                    try:
                        lv = re.findall('lv(\d+)\.', li.find('img', {'class': 'user-rank-rst '})['src'])[0]
                    except:
                        lv = re.findall('lv(\d+)\.', li.find('img', {'class': 'user-rank-rst'})['src'])[0]
                    if vip:
                        vip = '是'
                    else:
                        vip = '否'
                    for font_type in font_types:
                        try:
                            # print(11111)
                            name = li.find('a', {'class': 'name'}).text.strip()
                            place = font_dict[font_type]
                            # print(place)
                            true_font = get_ture_font(place)
                            # print(true_font)
                            re_str = re.search('<span class="{}"></span>'.format(font_type), sm).group()
                            sm = sm.replace(re_str, true_font)
                        except Exception as e:
                            # print(e)
                            continue
                        # break
                    sm = sm.strip().replace('\t', '').replace('\r\n', '').replace('\n', '')
                    try:
                        sm = re.search('<div class="review-words Hide">.*<div', sm).group().replace(
                            '<div class="review-words Hide">', '').replace('<div', '').strip().replace('<br/>', '')
                    except:
                        try:
                            sm = re.search('<div class="review-words">.*<.*?div', sm).group().replace(
                                '<div class="review-words">', '').replace('<div', '').strip().replace('<br/>',
                                                                                                      '').replace(
                                '</div', '')
                        except Exception as e:
                            print(e)
                            continue
                except Exception as e:
                    # print(e)
                    continue
                sm = re.sub('<img.*?>', ',', sm)
                print(name, pingfen, vip, xiaoguo, huanjing, fuwu, lv, tupian, shijian, dianzan, huiying, sm)
                worksheet.write(num, 0, name)
                worksheet.write(num, 1, pingfen)
                worksheet.write(num, 2, vip)
                worksheet.write(num, 3, xiaoguo)
                worksheet.write(num, 4, huanjing)
                worksheet.write(num, 5, fuwu)
                worksheet.write(num, 6, xiaofei)
                worksheet.write(num, 7, lv)
                worksheet.write(num, 8, tupian)
                worksheet.write(num, 9, shijian)
                worksheet.write(num, 10, dianzan)
                worksheet.write(num, 11, huiying)
                worksheet.write(num, 12, sm)
                num += 1
                # break
            if not re.search('下一页', str(soup)):
                break
            time.sleep(random.randint(1, 3))
            # break
    except Exception as e:
        print(e)
        s = ''

    print(url)
    print(soup)
    workbook.save('{}.xls'.format(id))
